# Sounds of Seismic (SOS): Implementation Schema
## Algorithmic Composition from MiniSeed Seismic Data

**Version:** 3.2  
**Channel:** BHZ (Broadband Vertical Motion)  
**Stack:** Pure JavaScript / Web Audio API  
**Reference Implementation:** ONE.html  
**Synthesis Date:** December 2025

---

## 1. System Architecture Overview

The SOS system transforms MiniSeed BHZ seismic waveform data into electronica music through algorithmic composition, implemented entirely in vanilla JavaScript with the Web Audio API. No external seismic parsing libraries required.

```
┌─────────────────────────────────────────────────────────────────────────┐
│                        LAYER 5: OUTPUT & RENDERING                      │
│                Web Audio API · Reverb · Delay · Master                  │
├─────────────────────────────────────────────────────────────────────────┤
│                       LAYER 4: AESTHETIC CONTROL                        │
│         Chord Progressions · Phrase Selection · Depth Mapping           │
├─────────────────────────────────────────────────────────────────────────┤
│                    LAYER 3: SOUND SYNTHESIS (MICRO)                     │
│            Granular Grains · Voice Pool · Pads · Bass Drone             │
├─────────────────────────────────────────────────────────────────────────┤
│                 LAYER 2: STRUCTURAL GENERATION (MACRO)                  │
│             Phrase Sequencing · Chord Changes · LFO Mixing              │
├─────────────────────────────────────────────────────────────────────────┤
│                  LAYER 1: DATA ACQUISITION & PARSING                    │
│          USGS Event Fetch · IRIS MiniSeed Fetch · Native Parse          │
└─────────────────────────────────────────────────────────────────────────┘
                                    ▲
                                    │
                    ┌───────────────┴───────────────┐
                    │     USGS Earthquake API       │
                    │     IRIS FDSN Dataselect      │
                    │         (BHZ Channel)         │
                    └───────────────────────────────┘
```

---

## 2. Layer 1: Data Acquisition & Parsing

### 2.1 Native MiniSeed Parser (No External Libraries)

The MiniSeed format can be parsed directly with JavaScript DataView. No external libraries required.

```javascript
function parseMiniSEED(buffer) {
    const view = new DataView(buffer);
    
    // Validate minimum header size
    if (buffer.byteLength < 64) {
        throw new Error("Buffer too small for MiniSEED header.");
    }
    
    // Extract sample count from fixed header (byte offset 46, big-endian)
    const numSamples = view.getInt16(46, false);
    
    // Data begins at offset 64 (standard MiniSEED fixed header length)
    const dataOffset = 64;
    
    // Validate buffer contains expected data
    if (numSamples <= 0 || buffer.byteLength < dataOffset + numSamples * 4) {
        throw new Error(`Invalid sample count (${numSamples}) or buffer size.`);
    }
    
    // Extract raw 32-bit integer samples
    const rawData = new Int32Array(buffer.slice(dataOffset, dataOffset + numSamples * 4));
    
    // Normalize to [-1, 1] range
    const maxVal = rawData.reduce((max, val) => Math.max(max, Math.abs(val)), 0);
    if (maxVal === 0) return new Float32Array(rawData.length);
    
    const factor = 1.0 / maxVal;
    const normalized = new Float32Array(rawData.length);
    for (let i = 0; i < rawData.length; i++) {
        normalized[i] = rawData[i] * factor;
    }
    
    return normalized;
}
```

**MiniSeed Header Structure (Relevant Fields):**

| Byte Offset | Field | Type | Description |
|-------------|-------|------|-------------|
| 0-5 | Sequence Number | ASCII | Record sequence |
| 6 | Data Quality | ASCII | D, R, Q, or M |
| 8-12 | Station Code | ASCII | e.g., "ANMO" |
| 13-14 | Location | ASCII | e.g., "00" |
| 15-17 | Channel | ASCII | e.g., "BHZ" |
| 18-19 | Network | ASCII | e.g., "IU" |
| 46-47 | Number of Samples | Int16 BE | Sample count |
| 64+ | Data | Int32 | Waveform samples |

### 2.2 USGS Earthquake Event Fetch

```javascript
async function fetchLatestEvent(minMagnitude = 6.0, timespanDays = 30) {
    const end = new Date().toISOString();
    const start = new Date(Date.now() - timespanDays * 24 * 60 * 60 * 1000).toISOString();
    
    const url = `https://earthquake.usgs.gov/fdsnws/event/1/query?` +
                `format=geojson&starttime=${start}&endtime=${end}` +
                `&minmagnitude=${minMagnitude}&orderby=time`;
    
    const res = await fetch(url);
    if (!res.ok) throw new Error(`USGS Event Fetch failed: ${res.status}`);
    
    const data = await res.json();
    if (!data.features || data.features.length === 0) {
        throw new Error(`No events found with M >= ${minMagnitude}`);
    }
    
    return data.features[0];  // Most recent event
}
```

**USGS GeoJSON Response Structure:**

| Field Path | Type | Description |
|------------|------|-------------|
| `features[0].properties.mag` | Number | Magnitude |
| `features[0].properties.place` | String | Location description |
| `features[0].properties.time` | Number | Unix timestamp (ms) |
| `features[0].geometry.coordinates[2]` | Number | Depth (km) |
| `features[0].id` | String | Event ID for USGS URL |

### 2.3 IRIS FDSN MiniSeed Fetch

```javascript
async function fetchMiniSEED(eventTime, config) {
    const { net, sta, loc, cha, duration } = config;
    
    // Start 5 seconds before event
    const startTime = new Date(new Date(eventTime).getTime() - 5000).toISOString();
    const endTime = new Date(new Date(startTime).getTime() + duration * 1000).toISOString();
    
    const url = `https://service.iris.edu/fdsnws/dataselect/1/query?` +
                `net=${net}&sta=${sta}&loc=${loc}&cha=${cha}` +
                `&starttime=${startTime}&endtime=${endTime}&format=miniseed`;
    
    const res = await fetch(url);
    if (!res.ok) throw new Error(`IRIS MiniSEED Fetch failed: ${res.status}`);
    if (res.status === 204) throw new Error(`No data returned from IRIS for station ${sta}.`);
    
    return await res.arrayBuffer();
}
```

**Default Station Configuration:**

```javascript
const STATION_CONFIG = {
    net: "IU",      // Global Seismographic Network
    sta: "ANMO",    // Albuquerque, New Mexico
    loc: "00",      // Primary sensor
    cha: "BHZ",     // Broadband High-gain Vertical
    duration: 60    // Seconds of data
};
```

### 2.4 Audio Buffer Creation

```javascript
function createAudioBufferFromFloat32Array(audioCtx, float32Array) {
    if (!audioCtx || !float32Array || float32Array.length === 0) return null;
    
    const buffer = audioCtx.createBuffer(1, float32Array.length, audioCtx.sampleRate);
    buffer.copyToChannel(float32Array, 0);
    return buffer;
}
```

---

## 3. Layer 2: Structural Generation (Macro)

### 3.1 Musical Phrase System

Melodic phrases defined as semitone intervals from root note:

```javascript
const SEISMIC_PHRASES = [
    [5, 7, 4, 2, 0, 12, 7, 5, 7, 4, 2, 0],
    [5, 7, 4, 2, 0, 12, 4, 7, 5, 0],
    [-5, 2, 0, 4, 7, 12, 5, 2, 7, 4, 0, 7, 2, 5, 5, 2, 4, 0],
    [7, 7, 2, 4, 4, 4, 2, 0, 7, 0, 0]
];

// Phrase changes every N seconds
const PHRASE_CHANGE_INTERVAL_S = 10;
```

### 3.2 Harmonic Chord System

Chords defined as semitone intervals from root:

```javascript
const PAD_CHORDS = [
    { name: "Cmaj7",      intervals: [0, 4, 7, 12] },
    { name: "Fmaj7#11",   intervals: [4, 7, 11, 16] },
    { name: "Abmaj7",     intervals: [-3, 0, 4, 7] },
    { name: "Abadd#11",   intervals: [-3, 0, 5, 9] }
];

// Chord changes every N seconds
const CHORD_CHANGE_INTERVAL_S = 30;
```

### 3.3 Root Note Mapping from Magnitude

Root note calculated as semitone offset, then converted to frequency:

```javascript
function calculateRootNote(magnitude, minMagnitude = 6.0, baseNote = 57) {
    // Higher magnitude = higher root pitch
    // Base: A3 (note 57), scaling by 2 semitones per magnitude unit
    return baseNote + Math.floor((magnitude - minMagnitude) * 2);
}

// Note number to frequency conversion (A4 = 440 Hz at note 69)
const ntof = (note) => 440 * Math.pow(2, (note - 69) / 12);
```

### 3.4 Depth-to-Effect Mapping

```javascript
function calculateDepthEffects(depth, maxDepth = 600) {
    const depthFactor = Math.min(1, depth / maxDepth);
    
    return {
        reverbWet: 0.2 + depthFactor * 0.5,      // Deeper = more reverb
        delayTime: 0.2 + depthFactor * 0.5,      // Deeper = longer delay
        delayFeedback: 0.3 + depthFactor * 0.3,  // Deeper = more feedback
        klankGain: 0.003 + (depth / 700) * 0.005 // Deeper = more resonance
    };
}
```

---

## 4. Layer 3: Sound Synthesis (Micro)

### 4.1 Bass Drone Engine

Sub-bass foundation using dual oscillators with slow LFO modulation:

```javascript
function startBassDrone(audioCtx, masterGain, rootNote) {
    const now = audioCtx.currentTime;
    const droneGain = audioCtx.createGain();
    droneGain.gain.value = 0.25;
    
    // Sub-bass oscillator (2 octaves below root)
    const osc1 = audioCtx.createOscillator();
    osc1.type = 'sine';
    osc1.frequency.value = ntof(rootNote - 24);
    
    // Slow amplitude LFO for breathing effect
    const lfo1 = audioCtx.createOscillator();
    lfo1.frequency.value = 0.005 + Math.random() * 0.005;  // 0.005-0.01 Hz
    const lfo1Gain = audioCtx.createGain();
    lfo1Gain.gain.value = 0.05;
    lfo1.connect(lfo1Gain).connect(droneGain.gain);
    
    // Bass oscillator (1 octave below root)
    const osc2 = audioCtx.createOscillator();
    osc2.type = 'sine';
    osc2.frequency.value = ntof(rootNote - 12);
    
    // Second LFO slightly faster
    const lfo2 = audioCtx.createOscillator();
    lfo2.frequency.value = 0.008 + Math.random() * 0.005;
    const lfo2Gain = audioCtx.createGain();
    lfo2Gain.gain.value = 0.02;
    lfo2.connect(lfo2Gain).connect(droneGain.gain);
    
    // Connect to output
    osc1.connect(droneGain);
    osc2.connect(droneGain);
    droneGain.connect(masterGain);
    
    // Start all oscillators
    osc1.start(now);
    lfo1.start(now);
    osc2.start(now);
    lfo2.start(now);
    
    return { droneGain, osc1, lfo1, lfo1Gain, osc2, lfo2, lfo2Gain };
}
```

### 4.2 Pad Synthesis with Klank Resonators

Sawtooth pads with noise-excited resonant filters (Klank):

```javascript
function startPads(audioCtx, masterGain, reverb, delay, rootNote, depth) {
    const now = audioCtx.currentTime;
    
    // Noise source for Klank excitation
    const noiseBuffer = audioCtx.createBuffer(1, audioCtx.sampleRate * 2, audioCtx.sampleRate);
    const noiseData = noiseBuffer.getChannelData(0);
    for (let i = 0; i < noiseData.length; i++) {
        noiseData[i] = Math.random() * 2 - 1;
    }
    
    const noiseSource = audioCtx.createBufferSource();
    noiseSource.buffer = noiseBuffer;
    noiseSource.loop = true;
    
    // Gain nodes
    const padMasterGain = audioCtx.createGain();
    padMasterGain.gain.value = 0.2;
    padMasterGain.connect(masterGain);
    if (reverb) padMasterGain.connect(reverb);
    if (delay) padMasterGain.connect(delay);
    
    const klankMasterGain = audioCtx.createGain();
    klankMasterGain.gain.value = 0.003 + (depth / 700) * 0.005;
    noiseSource.connect(klankMasterGain);
    if (reverb) klankMasterGain.connect(reverb);
    if (delay) klankMasterGain.connect(delay);
    
    let padOscs = [];
    let klankFilters = [];
    
    function setupChord(chord) {
        const notes = chord.intervals.map(i => rootNote + i);
        const t = audioCtx.currentTime;
        
        // Clean up previous oscillators
        padOscs.forEach(osc => { osc.stop(t + 0.1); osc.disconnect(); });
        klankFilters.forEach(f => f.disconnect());
        padOscs = [];
        klankFilters = [];
        
        // Create 8 detuned oscillators (4 notes × 2 octaves)
        for (let i = 0; i < 8; i++) {
            const osc = audioCtx.createOscillator();
            osc.type = 'sawtooth';
            const noteNum = notes[i % 4] + (i < 4 ? 0 : -12);
            osc.frequency.value = ntof(noteNum);
            osc.detune.value = (Math.random() - 0.5) * 8;  // Slight detune in cents
            
            const panner = audioCtx.createStereoPanner();
            panner.pan.value = Math.random() * 2 - 1;
            
            osc.connect(panner).connect(padMasterGain);
            osc.start(t);
            padOscs.push(osc);
        }
        
        // Create Klank resonators for each chord note
        notes.forEach(note => {
            const filter = audioCtx.createBiquadFilter();
            filter.type = 'bandpass';
            filter.frequency.value = ntof(note);
            filter.Q.value = 50;  // High Q for resonance
            klankMasterGain.connect(filter).connect(masterGain);
            klankFilters.push(filter);
        });
    }
    
    noiseSource.start(now);
    
    return { padMasterGain, klankMasterGain, noiseSource, setupChord, padOscs, klankFilters };
}
```

### 4.3 Melody Sequencer with Granular Crossfade

The core innovation: LFO-controlled crossfade between synthesized tones and granular seismic playback.

```javascript
function startMelodySequencer(audioCtx, masterGain, reverb, delay, seismicBuffer, rootNote) {
    let currentPhrase = getRandom(SEISMIC_PHRASES);
    let noteIndex = 0;
    let voiceIndex = 0;
    
    // --- Synth/Grain Mixer with LFO Crossfade ---
    const mixer = {
        synthGain: audioCtx.createGain(),
        granularGain: audioCtx.createGain(),
        lfo: audioCtx.createOscillator(),
        lfoGain: audioCtx.createGain(),
        inverter: audioCtx.createGain(),
        one: audioCtx.createConstantSource()
    };
    
    // LFO controls crossfade between synth and granular
    mixer.lfo.frequency.value = 0.03 + Math.random() * 0.02;  // 0.03-0.05 Hz
    mixer.lfoGain.gain.value = 0.5;
    mixer.inverter.gain.value = -1;
    mixer.one.offset.value = 1;
    
    // LFO → synthGain (positive)
    mixer.lfo.connect(mixer.lfoGain);
    mixer.lfoGain.connect(mixer.synthGain.gain);
    
    // LFO → granularGain (inverted)
    mixer.lfoGain.connect(mixer.inverter);
    mixer.one.connect(mixer.granularGain.gain);
    mixer.inverter.connect(mixer.granularGain.gain);
    
    // Master melody output
    const melodyMasterGain = audioCtx.createGain();
    melodyMasterGain.gain.value = 0.5;
    mixer.synthGain.connect(melodyMasterGain);
    mixer.granularGain.connect(melodyMasterGain);
    melodyMasterGain.connect(masterGain);
    if (reverb) melodyMasterGain.connect(reverb);
    if (delay) melodyMasterGain.connect(delay);
    
    mixer.one.start();
    mixer.lfo.start();
    
    // --- Voice Pool (Glitch-Free Performance) ---
    const NUM_VOICES = 6;
    const voices = [];
    
    for (let i = 0; i < NUM_VOICES; i++) {
        const osc = audioCtx.createOscillator();
        osc.type = 'triangle';
        
        const filter = audioCtx.createBiquadFilter();
        filter.type = 'lowpass';
        filter.Q.value = 4;
        
        const env = audioCtx.createGain();
        env.gain.value = 0;
        
        osc.connect(filter).connect(env).connect(mixer.synthGain);
        osc.start();
        
        const grainEnv = audioCtx.createGain();
        grainEnv.gain.value = 0;
        grainEnv.connect(mixer.granularGain);
        
        voices.push({ osc, filter, env, grainEnv });
    }
    
    // --- Note Playback Function ---
    function playMelodyNote() {
        const voice = voices[voiceIndex % NUM_VOICES];
        voiceIndex++;
        
        const noteNum = rootNote + currentPhrase[noteIndex % currentPhrase.length] - 12;
        const freq = ntof(noteNum);
        const now = audioCtx.currentTime;
        
        // Trigger synthesizer voice
        voice.osc.frequency.setTargetAtTime(freq, now, 0.01);
        voice.filter.frequency.setTargetAtTime(freq * 1.5, now, 0.01);
        voice.env.gain.cancelScheduledValues(now);
        voice.env.gain.setValueAtTime(voice.env.gain.value, now);
        voice.env.gain.linearRampToValueAtTime(1, now + 0.01);
        voice.env.gain.setTargetAtTime(0, now + 0.01, 1.5);
        
        // Trigger granular seismic grain
        if (seismicBuffer) {
            const grainSource = audioCtx.createBufferSource();
            grainSource.buffer = seismicBuffer;
            grainSource.playbackRate.value = freq;  // Pitch-shift grain to note frequency
            
            voice.grainEnv.gain.cancelScheduledValues(now);
            voice.grainEnv.gain.setValueAtTime(voice.grainEnv.gain.value, now);
            voice.grainEnv.gain.linearRampToValueAtTime(0.4, now + 0.02);
            voice.grainEnv.gain.setTargetAtTime(0, now + 0.02, 1.0);
            
            // Random grain position within buffer
            const grainOffset = Math.random() * (seismicBuffer.duration - 0.2);
            grainSource.connect(voice.grainEnv);
            grainSource.start(now, Math.max(0, grainOffset), 0.15);  // 150ms grain
            
            grainSource.onended = () => { try { grainSource.disconnect(); } catch(e){} };
        }
        
        noteIndex++;
        
        // Schedule next note with variable timing
        const randomRate = (getRandom([0.5, 1, 2, 4]) / 2) * (0.8 + Math.random() * 0.4);
        return setTimeout(playMelodyNote, (1 / randomRate) * 1000);
    }
    
    return { mixer, voices, melodyMasterGain, playMelodyNote };
}
```

**Granular Grain Parameters:**

| Parameter | Value | Derivation |
|-----------|-------|------------|
| Grain Duration | 150ms | Fixed short grain |
| Grain Position | Random | `Math.random() * (buffer.duration - 0.2)` |
| Playback Rate | Note Frequency (Hz) | Direct frequency as playbackRate |
| Envelope Attack | 20ms | Fast attack |
| Envelope Decay | ~1s | Exponential decay |

### 4.4 Utility Functions

```javascript
// Note number to Frequency conversion (A4 = 440 Hz at note 69)
const ntof = (note) => 440 * Math.pow(2, (note - 69) / 12);

// Random array element selection
const getRandom = (arr) => arr[Math.floor(Math.random() * arr.length)];
```

---

## 5. Layer 4: Aesthetic Control

### 5.1 Seismic-to-Musical Parameter Mapping

| Seismic Parameter | Musical Parameter | Mapping Function |
|-------------------|-------------------|------------------|
| Magnitude | Root Note | `57 + (mag - 6.0) * 2` → frequency via `ntof()` |
| Depth | Reverb Wet | `0.2 + (depth/600) * 0.5` |
| Depth | Delay Time | `0.2 + (depth/600) * 0.5` |
| Depth | Delay Feedback | `0.3 + (depth/600) * 0.3` |
| Depth | Klank Resonance | `0.003 + (depth/700) * 0.005` |
| Waveform | Grain Source | Direct buffer playback |

### 5.2 Temporal Event Scheduling

```javascript
const TIMING_CONFIG = {
    phraseChange: 10,    // seconds between phrase changes
    chordChange: 30,     // seconds between chord changes
    padSwell: 10,        // seconds for pad envelope
    masterFadeIn: 5.0    // seconds for initial fade-in
};
```

### 5.3 Chord Progression Logic

```javascript
function scheduleChordChanges(setupChord, padMasterGain, isPlaying) {
    function changeChord() {
        if (!isPlaying) return;
        
        const chord = getRandom(PAD_CHORDS);
        const t = audioCtx.currentTime;
        
        // Fade out, change, fade in
        padMasterGain.gain.cancelScheduledValues(t);
        padMasterGain.gain.setTargetAtTime(0.01, t, 0.1);
        padMasterGain.gain.setTargetAtTime(0.2, t + 0.5, PAD_SWELL_TIME_S / 2);
        
        setupChord(chord);
    }
    
    changeChord();  // Initial chord
    return setInterval(changeChord, CHORD_CHANGE_INTERVAL_S * 1000);
}
```

---

## 6. Layer 5: Output & Rendering

### 6.1 Effects Chain

#### Convolution Reverb (Algorithmic Impulse)

```javascript
async function createReverb(audioCtx, masterGain) {
    const impulseLength = 2.5 * audioCtx.sampleRate;
    const impulseBuffer = audioCtx.createBuffer(2, impulseLength, audioCtx.sampleRate);
    
    // Generate exponentially decaying noise impulse
    for (let channel = 0; channel < 2; channel++) {
        const data = impulseBuffer.getChannelData(channel);
        for (let i = 0; i < impulseLength; i++) {
            data[i] = (Math.random() * 2 - 1) * Math.pow(1 - i / impulseLength, 3.0);
        }
    }
    
    const reverb = audioCtx.createConvolver();
    reverb.buffer = impulseBuffer;
    
    const reverbWetGain = audioCtx.createGain();
    reverbWetGain.gain.value = 0.4;
    
    reverb.connect(reverbWetGain).connect(masterGain);
    
    return { reverb, reverbWetGain };
}
```

#### Feedback Delay

```javascript
function createDelay(audioCtx, masterGain) {
    const delay = audioCtx.createDelay(2.0);
    delay.delayTime.value = 0.45;
    
    const delayFeedbackGain = audioCtx.createGain();
    delayFeedbackGain.gain.value = 0.5;
    
    const delayWetGain = audioCtx.createGain();
    delayWetGain.gain.value = 0.35;
    
    // Feedback loop
    delay.connect(delayFeedbackGain).connect(delay);
    delay.connect(delayWetGain).connect(masterGain);
    
    return { delay, delayFeedbackGain, delayWetGain };
}
```

### 6.2 Master Signal Flow

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│  Bass Drone │────▶│             │     │             │
└─────────────┘     │             │     │             │
                    │   Master    │────▶│ Destination │
┌─────────────┐     │    Gain     │     │  (Output)   │
│    Pads     │────▶│             │     │             │
└─────────────┘     │             │     │             │
        │           └─────────────┘     └─────────────┘
        │                  ▲
        ▼                  │
┌─────────────┐     ┌──────┴──────┐
│   Reverb    │────▶│  Reverb Wet │
└─────────────┘     └─────────────┘
        │
        ▼
┌─────────────┐     ┌─────────────┐
│   Delay     │────▶│  Delay Wet  │────────────┐
└─────────────┘     └─────────────┘            │
        ▲                                       │
        │                                       ▼
        └───────────────────────────────[Feedback Loop]


┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Melody    │     │ LFO Xfade   │     │   Melody    │
│   Synth     │────▶│  Mix        │────▶│   Master    │────▶ [Master Gain]
│   Voices    │     │             │     │             │
└─────────────┘     └─────────────┘     └─────────────┘
                           ▲
┌─────────────┐            │
│  Granular   │────────────┘
│  Grains     │
│ (Seismic)   │
└─────────────┘
```

### 6.3 Node Lifecycle Management

```javascript
// Track all active nodes for cleanup
let activeAudioNodes = [];
let activeSchedulers = [];

function stopSonification() {
    isPlaying = false;
    
    // Clear all schedulers
    activeSchedulers.forEach(id => {
        clearInterval(id);
        clearTimeout(id);
    });
    activeSchedulers = [];
    
    if (audioCtx) {
        const now = audioCtx.currentTime;
        
        // Fade out master
        masterGain.gain.setTargetAtTime(0, now, 0.5);
        
        // Stop all oscillators
        activeAudioNodes.forEach(node => {
            if (node.stop) node.stop(now + 0.6);
        });
        
        // Disconnect after fade
        setTimeout(() => {
            activeAudioNodes.forEach(node => {
                try { node.disconnect(); } catch (e) {}
            });
            activeAudioNodes = [];
            audioCtx.close();
            audioCtx = null;
        }, 1000);
    }
}
```

---

## 7. Complete Implementation Template

### 7.1 Configuration Constants

```javascript
// Data Fetching
const SEISMIC_DATA_DURATION_S = 60;
const MIN_MAGNITUDE = 6.0;
const FETCH_TIMESPAN_DAYS = 30;
const SAMPLE_RATE = 44100;

// Station
const STATION_INFO = { net: "IU", sta: "ANMO", loc: "00", cha: "BHZ" };

// Musical
const PHRASE_CHANGE_INTERVAL_S = 10;
const CHORD_CHANGE_INTERVAL_S = 30;
const PAD_SWELL_TIME_S = 10;
const KLANK_RESONANCE_Q = 50;
```

### 7.2 Main Startup Sequence

```javascript
async function startSonification() {
    // 1. Initialize AudioContext
    audioCtx = new AudioContext({ sampleRate: SAMPLE_RATE });
    await audioCtx.resume();
    
    // 2. Create master gain (start at 0 for fade-in)
    masterGain = audioCtx.createGain();
    masterGain.gain.value = 0;
    masterGain.connect(audioCtx.destination);
    
    // 3. Create effects
    const { reverb, reverbWetGain } = await createReverb(audioCtx, masterGain);
    const { delay, delayFeedbackGain, delayWetGain } = createDelay(audioCtx, masterGain);
    
    // 4. Fetch earthquake event
    const event = await fetchLatestEvent(MIN_MAGNITUDE, FETCH_TIMESPAN_DAYS);
    const quakeInfo = {
        place: event.properties.place,
        mag: event.properties.mag,
        depth: event.geometry.coordinates[2],
        time: new Date(event.properties.time).toISOString()
    };
    
    // 5. Fetch and parse MiniSeed
    const miniSeedBuffer = await fetchMiniSEED(quakeInfo.time, STATION_INFO);
    const rawSamples = parseMiniSEED(miniSeedBuffer);
    const seismicAudioBuffer = createAudioBufferFromFloat32Array(audioCtx, rawSamples);
    
    // 6. Calculate musical parameters from seismic data
    const rootNote = 57 + Math.floor((quakeInfo.mag - MIN_MAGNITUDE) * 2);
    const depthEffects = calculateDepthEffects(quakeInfo.depth);
    
    // 7. Apply depth-based effect parameters
    reverbWetGain.gain.setTargetAtTime(depthEffects.reverbWet, audioCtx.currentTime, 0.5);
    delay.delayTime.setTargetAtTime(depthEffects.delayTime, audioCtx.currentTime, 0.5);
    delayFeedbackGain.gain.setTargetAtTime(depthEffects.delayFeedback, audioCtx.currentTime, 0.5);
    
    // 8. Start synthesis engines
    isPlaying = true;
    startBassDrone(audioCtx, masterGain, rootNote);
    startPads(audioCtx, masterGain, reverb, delay, rootNote, quakeInfo.depth);
    startMelodySequencer(audioCtx, masterGain, reverb, delay, seismicAudioBuffer, rootNote);
    
    // 9. Fade in master
    masterGain.gain.setTargetAtTime(0.6, audioCtx.currentTime, 5.0);
}
```

---

## 8. Kolmogorov Complexity Integration (Extension)

To integrate AIT-driven complexity analysis with the ONE.html approach:

### 8.1 Compression-Based K(x) Estimation

```javascript
async function estimateKolmogorov(samples) {
    // Convert Float32Array to bytes for compression
    const bytes = new Uint8Array(samples.buffer);
    
    // Use CompressionStream API (native browser)
    const cs = new CompressionStream('gzip');
    const writer = cs.writable.getWriter();
    writer.write(bytes);
    writer.close();
    
    const compressedChunks = [];
    const reader = cs.readable.getReader();
    
    while (true) {
        const { done, value } = await reader.read();
        if (done) break;
        compressedChunks.push(value);
    }
    
    const compressedLength = compressedChunks.reduce((sum, chunk) => sum + chunk.length, 0);
    return compressedLength / bytes.length;  // Compression ratio
}
```

### 8.2 Complexity-Driven Phrase Selection

```javascript
async function selectPhraseByComplexity(seismicBuffer) {
    const samples = seismicBuffer.getChannelData(0);
    const kScore = await estimateKolmogorov(samples);
    
    // Map complexity to phrase index
    // Low K = simple phrases, High K = complex phrases
    const phraseIndex = Math.floor(kScore * SEISMIC_PHRASES.length);
    return SEISMIC_PHRASES[Math.min(phraseIndex, SEISMIC_PHRASES.length - 1)];
}
```

### 8.3 Dynamic Complexity Windowing

```javascript
function analyzeWindowedComplexity(samples, windowSize = 4096) {
    const windows = [];
    
    for (let i = 0; i < samples.length - windowSize; i += windowSize) {
        const window = samples.slice(i, i + windowSize);
        windows.push({
            start: i,
            end: i + windowSize,
            complexity: quickEntropyEstimate(window)
        });
    }
    
    return windows;
}

function quickEntropyEstimate(samples) {
    // Shannon entropy approximation
    const histogram = new Array(256).fill(0);
    const scale = 127.5;
    
    for (const sample of samples) {
        const bin = Math.floor((sample + 1) * scale);
        histogram[Math.max(0, Math.min(255, bin))]++;
    }
    
    let entropy = 0;
    const total = samples.length;
    
    for (const count of histogram) {
        if (count > 0) {
            const p = count / total;
            entropy -= p * Math.log2(p);
        }
    }
    
    return entropy / 8;  // Normalize to 0-1
}
```

---

## 9. Parameter Quick Reference

### Seismic-to-Audio Mapping (BHZ Channel)

| Seismic | Audio | Formula |
|---------|-------|---------|
| Magnitude | Root Note → Frequency | `ntof(57 + (mag - 6) * 2)` |
| Depth | Reverb Wet | `0.2 + (depth/600) * 0.5` |
| Depth | Delay Time | `0.2 + (depth/600) * 0.5` |
| Depth | Feedback | `0.3 + (depth/600) * 0.3` |
| Depth | Klank Gain | `0.003 + (depth/700) * 0.005` |
| Waveform | Grain Pitch | Frequency (Hz) as playbackRate |
| Waveform | Grain Position | Random offset in buffer |

### Synthesis Parameters

| Component | Parameter | Value |
|-----------|-----------|-------|
| Bass Drone | Oscillator Type | Sine |
| Bass Drone | Octaves Below Root | -24, -12 semitones |
| Bass Drone | LFO Rate | 0.005-0.013 Hz |
| Pads | Oscillator Type | Sawtooth |
| Pads | Voice Count | 8 (4 notes × 2 octaves) |
| Pads | Detune | ±4 cents |
| Klank | Filter Type | Bandpass |
| Klank | Q | 50 |
| Melody | Oscillator Type | Triangle |
| Melody | Voice Pool Size | 6 |
| Melody | Filter Type | Lowpass, Q=4 |
| Grain | Duration | 150ms |
| Grain | Envelope Attack | 20ms |
| Grain | Envelope Decay | ~1s |

### Note-to-Frequency Reference

```javascript
// Note number to frequency (Hz)
// Note 69 = A4 = 440 Hz
const ntof = (note) => 440 * Math.pow(2, (note - 69) / 12);

// Example frequencies:
// Note 57 (A3)  = 220.00 Hz
// Note 60 (C4)  = 261.63 Hz
// Note 69 (A4)  = 440.00 Hz
// Note 72 (C5)  = 523.25 Hz
```

---

## 10. Conclusion

This schema documents the proven approach implemented in ONE.html:

- **No external seismic libraries** — native JavaScript MiniSeed parsing via DataView
- **Pure Web Audio API** — all synthesis using native browser audio nodes
- **Direct frequency calculations** — note numbers converted to Hz via `ntof()`
- **Real USGS/IRIS data** — live earthquake events and waveforms
- **Seismic-to-musical mapping** — magnitude→pitch, depth→effects
- **Granular seismic grains** — waveform segments as pitched melodic elements
- **LFO crossfade mixing** — smooth transition between synth and granular voices

**The Earth provides the data. The browser provides the instrument. The algorithm translates geology into electronica.**

---

*Schema v3.2 — Based on ONE.html Reference Implementation*  
*Pure JavaScript / Native MiniSeed Parsing / Web Audio API*  
*Reference: Sounds of Seismic Project — sos.allshookup.org*
