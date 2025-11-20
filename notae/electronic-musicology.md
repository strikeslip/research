# Electronic Musicology: Theory, Synthesis, & Code
**A Comprehensive Breakdown of Electronic Lineages, Compositional Theory, and JavaScript Implementation**

---

## Table of Contents
1. [The Sonic Lineages: Artist Breakdown](#i-the-sonic-lineages-artist-breakdown)
2. [The Listening Curriculum](#ii-the-listening-curriculum)
3. [Engineering the Sound: Web Audio API Implementations](#iii-engineering-the-sound-web-audio-api-implementations)

---

## I. The Sonic Lineages: Artist Breakdown

A breakdown of style, synthesis techniques, and compositional theory across the history of electronic sound.

### 1. The Architects: Academic & Modular Pioneers
*Establishing the physics of sound, moving from tape to voltage.*

* **[Iannis Xenakis](https://en.wikipedia.org/wiki/Iannis_Xenakis)**
    * **Style:** Stochastic Music, [Musique Concrète](https://en.wikipedia.org/wiki/Musique_concr%C3%A8te).
    * **Technique:** Granular Synthesis (proto) & [UPIC](https://en.wikipedia.org/wiki/UPIC) system.
    * **Theory:** **Game Theory/Probability.** Sound is treated as a mass of particles rather than notes.
* **[Max Mathews](https://en.wikipedia.org/wiki/Max_Mathews)**
    * **Style:** Computer Music.
    * **Technique:** Created [MUSIC-N](https://en.wikipedia.org/wiki/MUSIC-N) languages. Pioneered Digital Wavetable Synthesis.
    * **Theory:** **The Sampling Theorem.** Proved any sound could be reconstructed numerically.
* **[Curtis Roads](https://en.wikipedia.org/wiki/Curtis_Roads)**
    * **Style:** Microsound.
    * **Technique:** Granular Synthesis authority.
    * **Theory:** **[Pulsar Synthesis](https://en.wikipedia.org/wiki/Granular_synthesis).** Manipulating sound at the micro time scale (1–100ms).
* **[Wendy Carlos](https://en.wikipedia.org/wiki/Wendy_Carlos)**
    * **Style:** Switched-On Classical.
    * **Technique:** [Moog synthesizer](https://en.wikipedia.org/wiki/Moog_synthesizer), Additive layering on subtractive gear.
    * **Theory:** **Klangfarbenmelodie.** Prioritizing timbral realization of orchestral scores.
* **[Suzanne Ciani](https://en.wikipedia.org/wiki/Suzanne_Ciani)**
    * **Style:** New Age, Buchla Experimentalism.
    * **Technique:** [Buchla 200 Series](https://en.wikipedia.org/wiki/Buchla_Electronic_Musical_Instruments), Quadraphonic spatial movement.
    * **Theory:** **The Wave as Organism.** Performing without a keyboard, focusing on Control Voltage (CV).

### 2. The Minimalists & Tape Manipulators
*Process-based composition altering the perception of time.*

* **[Steve Reich](https://en.wikipedia.org/wiki/Steve_Reich)** & **[Terry Riley](https://en.wikipedia.org/wiki/Terry_Riley)**
    * **Style:** Minimalism.
    * **Technique:** Tape Loops, Phasing, Time Lag Accumulators.
    * **Theory:** **Phase Shifting.** Composition is the observation of repeating patterns moving out of sync.
* **[Brian Eno](https://en.wikipedia.org/wiki/Brian_Eno)** & **[Robert Fripp](https://en.wikipedia.org/wiki/Robert_Fripp)**
    * **Style:** Ambient, Frippertronics.
    * **Technique:** [Generative music](https://en.wikipedia.org/wiki/Generative_music), Reel-to-Reel delay loops.
    * **Theory:** **Environmental Tinting.** Music that is "as ignorable as it is interesting."

### 3. Kosmische, Krautrock & The Synth Pop Explosion
*The integration of sequencers to create "Robot Pop."*

* **[Kraftwerk](https://en.wikipedia.org/wiki/Kraftwerk)**
    * **Style:** Synth-pop, Proto-Techno.
    * **Technique:** Custom sequencers, [Vocoders](https://en.wikipedia.org/wiki/Vocoder).
    * **Theory:** **Industrielle Volksmusik.** The "Man-Machine"—removing the ego from performance.
* **[Tangerine Dream](https://en.wikipedia.org/wiki/Tangerine_Dream)** & **[Michael Garrison](https://en.wikipedia.org/wiki/Michael_Garrison_(musician))**
    * **Style:** [Berlin School](https://en.wikipedia.org/wiki/Berlin_School_of_electronic_music).
    * **Technique:** Moog 960 Sequencers, Ratcheting.
    * **Theory:** **Improvisation over Pulse.** Fluid experimentation over a rigid sequence.
* **[Jean-Michel Jarre](https://en.wikipedia.org/wiki/Jean-Michel_Jarre)**
    * **Style:** Mega-event Electronic.
    * **Technique:** Arp 2600, Laser Harp.
    * **Theory:** **Phased Space.** Melodic simplicity supported by vast, phased string pads.
* **[Giorgio Moroder](https://en.wikipedia.org/wiki/Giorgio_Moroder)**
    * **Style:** Italo Disco.
    * **Technique:** Moog Modular Basslines.
    * **Theory:** **The Click.** Synchronizing the synth to the drum track for quantized 16th-note drive.

### 4. IDM & The "Warp" Sound
*Breaking the grid, chaos, and heavy digital processing.*

* **[Aphex Twin](https://en.wikipedia.org/wiki/Aphex_Twin)**
    * **Style:** [IDM](https://en.wikipedia.org/wiki/Intelligent_dance_music), Acid, Drill 'n' Bass.
    * **Technique:** Modified TB-303, Microtuning, Spectrogram embedding.
    * **Theory:** **Algorithmic Pranksterism.** Physically impossible drum programming.
* **[Autechre](https://en.wikipedia.org/wiki/Autechre)**
    * **Style:** Glitch, Generative.
    * **Technique:** [Max (software)](https://en.wikipedia.org/wiki/Max_(software)) (custom patches).
    * **Theory:** **System-Based Composition.** Creating rules and curating the output; focusing on the "error."
* **[Boards of Canada](https://en.wikipedia.org/wiki/Boards_of_Canada)**
    * **Style:** [Hauntology](https://en.wikipedia.org/wiki/Hauntology_(music)).
    * **Technique:** Tape Saturation, resampling VHS.
    * **Theory:** **Nostalgia as Texture.** Deliberate degradation to mimic false memories.
* **[Skee Mask](https://en.wikipedia.org/wiki/Skee_Mask)**
    * **Style:** Breakbeat, Ambient Techno.
    * **Technique:** Granular processing of vintage Jungle breaks.
    * **Theory:** **Deconstructed Rhythm.** Focusing on the "air" between drum hits.

### 5. Cinematic & Narrative Synthesis
*Sound designed to evoke visual imagery and tension.*

* **[John Carpenter](https://en.wikipedia.org/wiki/John_Carpenter)**
    * **Style:** Horror Synth.
    * **Technique:** Prophet-5, ARP Quadra.
    * **Theory:** **Minimalism for Tension.** Simple sawtooth basslines in 5/4 time.
* **[Disasterpeace](https://en.wikipedia.org/wiki/Disasterpeace)** & **[Ben Prunty](https://en.wikipedia.org/wiki/Ben_Prunty)**
    * **Style:** Chiptune, Neo-Score.
    * **Technique:** [FM Synthesis](https://en.wikipedia.org/wiki/Frequency_modulation_synthesis), Retro-emulation.
    * **Theory:** **Limitation as Aesthetic.** Using 8-bit constraints for emotional depth.

### 6. The Modern Vanguard
*High-definition digital sculpting and modular grids.*

* **[Oneohtrix Point Never (OPN)](https://en.wikipedia.org/wiki/Oneohtrix_Point_Never)**
    * **Style:** Plunderphonics, Vaporwave.
    * **Technique:** Juno-60, Aggressive Sampling.
    * **Theory:** **Ecological Sampling.** Treating pop culture sounds as "junk objects" to be sculpted.
* **[Sophie](https://en.wikipedia.org/wiki/Sophie_(musician))**
    * **Style:** PC Music, Hyperpop.
    * **Technique:** Elektron Monomachine, [Physical modeling synthesis](https://en.wikipedia.org/wiki/Physical_modeling_synthesis).
    * **Theory:** **Hyper-Reality.** Synthesizing textures that sound "more plastic than plastic."
* **[Richard Devine](https://en.wikipedia.org/wiki/Richard_Devine)**
    * **Style:** Glitch, Sound Design.
    * **Technique:** [Eurorack](https://en.wikipedia.org/wiki/Eurorack) Modular.
    * **Theory:** **Generative Patching.** Music for machines, by machines.

### 7. Industrial, Noise & Lo-Fi
*The darker, grittier side of the spectrum.*

* **[Fuck Buttons](https://en.wikipedia.org/wiki/Fuck_Buttons)** & **[Health](https://en.wikipedia.org/wiki/Health_(band))**
    * **Style:** Noise Pop.
    * **Technique:** Circuit-bent toys, Distortion.
    * **Theory:** **The Wall of Sound.** Overwhelming density to bury melody.
* **[Legowelt](https://en.wikipedia.org/wiki/Legowelt)**
    * **Style:** Lo-Fi Techno.
    * **Technique:** [Amiga Tracker](https://en.wikipedia.org/wiki/Music_tracker), Vintage digital gear.
    * **Theory:** **Smudging.** Embracing digital hiss and noise for atmosphere.

---

## II. The Listening Curriculum

A curated playlist to isolate and listen to the specific theories described above.

1.  **The Architects:** [Iannis Xenakis – "Concret PH"](https://en.wikipedia.org/wiki/Concret_PH)
    * *Listen for:* Granular synthesis. The sound of burning charcoal treated as a cloud of density.
2.  **The Minimalists:** [Steve Reich – "It’s Gonna Rain"](https://en.wikipedia.org/wiki/It%27s_Gonna_Rain)
    * *Listen for:* Phase Shifting. Two identical tape loops drifting apart to create new rhythms.
3.  **Kosmische:** [Kraftwerk – "The Robots"](https://en.wikipedia.org/wiki/The_Man-Machine)
    * *Listen for:* The Grid. The absolute rigidity of timing and vocoded vocals removing the "human."
4.  **IDM:** [Aphex Twin – "Vordhosbn"](https://en.wikipedia.org/wiki/Drukqs)
    * *Listen for:* Micro-editing. Rapid-fire snare rolls that are physically impossible to play (Tracker sequencing).
5.  **Cinematic:** [John Carpenter – "Halloween Theme"](https://en.wikipedia.org/wiki/Halloween_(1978_film)#Music)
    * *Listen for:* Subtractive Minimalism. Raw sawtooth waves and 5/4 time creating anxiety.
6.  **Modern Vanguard:** [Sophie – "Lemonade"](https://en.wikipedia.org/wiki/Product_(Sophie_album))
    * *Listen for:* Hyper-Reality. Synthesized bubbles using Physical Modeling/FM synthesis.
7.  **Industrial:** [Fuck Buttons – "Surf Solar"](https://en.wikipedia.org/wiki/Tarot_Sport)
    * *Listen for:* Saturation. The use of distortion as the primary instrument.

---

## III. Engineering the Sound: Web Audio API Implementations

How to replicate these historical techniques using code. Copy and paste these snippets into your browser console (F12).

---

### 1. The "Steve Reich" Phase Shifter
**Concept:** *Time Manipulation / Phase Shifting*
The listener hears two identical patterns drift apart. One loop is fixed at 1.0 seconds, the other loop is slightly longer (1.01s).



```javascript
// 1. Initialize the Studio
const ctx = new (window.AudioContext || window.webkitAudioContext)();

function playTone(time, panPos) {
    const osc = ctx.createOscillator();
    const gain = ctx.createGain();
    const panner = ctx.createStereoPanner();

    osc.type = 'sine';
    osc.frequency.value = 440; 

    // Short pluck envelope
    gain.gain.setValueAtTime(0, time);
    gain.gain.linearRampToValueAtTime(0.5, time + 0.01); 
    gain.gain.exponentialRampToValueAtTime(0.001, time + 0.2); 

    panner.pan.value = panPos;

    osc.connect(gain);
    gain.connect(panner);
    panner.connect(ctx.destination);

    osc.start(time);
    osc.stop(time + 0.3);
}

// 2. The "Reich" Logic
let startTime = ctx.currentTime + 0.5;
let iterations = 50; 

for (let i = 0; i < iterations; i++) {
    // LOOP 1: Left Ear (Static 1.0s interval)
    playTone(startTime + (i * 0.25), -1); 
    
    // LOOP 2: Right Ear (Phasing 1.01s interval)
    // The slight delay creates the phase shift
    playTone(startTime + (i * 0.255), 1); 
}
console.log("Steve Reich process started...");

2. The "SOPHIE" Bubble

Concept: FM Synthesis / Physical Modeling To get a "wet," elastic sound, we modulate the frequency of a Carrier oscillator using a Modulator oscillator. The modGain determines how "metallic" or "stretched" the material sounds.

Shutterstock
JavaScript

const ctx2 = new (window.AudioContext || window.webkitAudioContext)();

function triggerBubble() {
    const now = ctx2.currentTime;

    // The Carrier (The sound you hear)
    const carrier = ctx2.createOscillator();
    carrier.type = 'sine';
    carrier.frequency.value = 150; 

    // The Modulator (The "Wiggle")
    const modulator = ctx2.createOscillator();
    modulator.type = 'sine';
    modulator.frequency.value = 400; 

    // Modulation Depth
    const modGain = ctx2.createGain();
    const masterGain = ctx2.createGain();

    // PATCHING: Modulator -> ModGain -> Carrier Frequency (FM)
    modulator.connect(modGain);
    modGain.connect(carrier.frequency); 
    carrier.connect(masterGain);
    masterGain.connect(ctx2.destination);

    // ENVELOPES (Physics simulation)
    // 1. Pitch Drop (Bubble pop)
    carrier.frequency.setValueAtTime(300, now);
    carrier.frequency.exponentialRampToValueAtTime(50, now + 0.5);

    // 2. Timbre Morph (Metal to Sine)
    modGain.gain.setValueAtTime(1000, now); // High distortion start
    modGain.gain.exponentialRampToValueAtTime(1, now + 0.1); 

    // 3. Amplitude
    masterGain.gain.setValueAtTime(1, now);
    masterGain.gain.exponentialRampToValueAtTime(0.01, now + 0.5);

    carrier.start(now);
    modulator.start(now);
    carrier.stop(now + 0.5);
    modulator.stop(now + 0.5);
}

// Trigger multiple bubbles
triggerBubble();
setTimeout(triggerBubble, 200);
setTimeout(triggerBubble, 500);

3. The "Brian Eno" Generative Logic

Concept: Generative Probability / Ambient Systems Instead of writing a fixed melody, the composer writes a system of rules. We use Math.random() as a "Probability Gate" to decide IF a note plays, and what property that note has.

Getty Images
JavaScript

function generativeLoop() {
    // 30% chance to play a note
    if (Math.random() > 0.7) {
        console.log("Eno Event Triggered");
        // (Assuming playTone function from Section 1 exists)
        // playTone(ctx.currentTime, Math.random() * 2 - 1); 
    }
    // Recursive call with random timing
    setTimeout(generativeLoop, Math.random() * 500); 
}
// generativeLoop(); // Uncomment to run
