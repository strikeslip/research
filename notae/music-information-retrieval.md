## MIR (Music Information Retrieval) & AGS (Advanced Granular Synthesis)<br>
## Deep Research: Principles, Techniques & Application to SOS / SEISCLAW

---

## 1. WHO IS CRISTIAN VOGEL IN THIS CONTEXT

Cristian Vogel (b. 1972, Santiago de Chile) is not just an electronic music pioneer — he is a **practitioner-engineer** who has spent three decades building his own instruments before using them. He fled Pinochet's Chile as a child, grew up in Brighton UK, and became one of the founding voices of European techno via Tresor and Mille Plateaux in the early 1990s. What distinguishes him from most electronic musicians is that his artistic practice and his instrument-building practice are inseparable.

His studio label **NeverEngineLabs** (Copenhagen, est. 2013) is the formal expression of this — releasing tools, not just music. He is a **Kyma master** (Symbolic Sound's signal-domain programming environment, widely considered the most powerful real-time audio computation platform available), a Rust programmer, a JUCE developer, a Svelte/WebGL coder, and now the author of **Incline: Toposonic Corpus Explorer** — his most technically ambitious instrument to date, and the direct subject of this research.

His philosophy, stated plainly: *"The computers I work with are doing a better job of figuring out what notes are coming next than I probably could."* He is not interested in random generation. He is interested in **machine listening** — systems that understand what they are hearing and make decisions accordingly.

---

## 2. THE INSTRUMENT: INCLINE — TOPOSONIC CORPUS EXPLORER

Released March 2025 via NeverEngineLabs. Built in **Rust + JUCE**, wrapping the **FluCoMa C++ core libraries**. macOS first; Windows in development.

### Core Concept: The Corpus as Terrain

Incline's central metaphor is **topographic**: audio is not a linear sequence to be played back — it is a **navigable sonic terrain**. The instrument:

1. Ingests a fixed audio file (the corpus)
2. Runs MIR analysis to segment it into meaningful units
3. Maps those units into a multi-dimensional feature space
4. Reduces that space to a navigable 2D/3D map (the "toposonic" surface)
5. Lets the performer **traverse** that terrain in multiple modes

This is critically different from conventional granular synthesis. In standard granulars (GrainFrame, Granulator II, etc.), segmentation is a pre-processing step — you slice the audio, then forget the slices came from anywhere. In Incline, **segmentation and traversal are unified**: how you cut the corpus determines the topology of the terrain you explore.

### Three Runtime Modes

- **One Shot** — direct segment audition; precise oneshot and ratchet-style playback. Closest to traditional sample-triggering but driven by analysis position, not MIDI note.
- **One Loop** — continuous traversal with micro-loop continuity. The engine navigates across the terrain and loops semantically adjacent segments, creating evolving sustained textures.
- **Cloud** — distributed granular scheduling. Multiple grains scheduled from different terrain positions simultaneously, producing dense, evolving cloud textures whose character is determined by the topology of the analysis space around the current position.

All three modes share a **Contour Sequencer** — an autonomous traversal system that moves through the terrain on programmed or generative paths, synchronized to tempo or free-running.

---

## 3. THE TECHNICAL STACK: FLUCOMA

### What FluCoMa Is

**Fluid Corpus Manipulation** — a research project from the University of Huddersfield's HISS group, delivering a C++ core library (open source) with bindings for Max/MSP, SuperCollider, and Pure Data. Vogel wraps the C++ core directly in Rust/JUCE for Incline, bypassing the visual programming environments entirely.

FluCoMa's stated mission: *"bringing breakthroughs of signal decomposition DSP and machine learning to the toolset of techno-fluent computer composers."*

### The MIR Descriptor Set

FluCoMa provides the following audio descriptors (the "listening vocabulary" the engine uses):

**Timbral / Spectral:**
- `SpectralShape` — centroid, spread, skewness, kurtosis, rolloff, flatness, crest (7 values describing the shape of the spectrum)
- `MFCCs` — Mel-Frequency Cepstral Coefficients (typically 13–40 coefficients representing timbral fingerprint)
- `SpectralFlatness` — measure of noisiness vs. tonality
- `Pitch` — fundamental frequency + confidence value
- `Loudness` — perceptual loudness in LUFS

**Temporal / Structural:**
- `AmpSlice` — onset detection based on amplitude envelope changes
- `OnsetSlice` — spectral flux onset detection (detects timbral change, not just loudness)
- `TransientSlice` — isolates transient attacks from sustained components
- `NoveltySlice` — self-similarity matrix approach; detects where audio is *structurally different* from its recent past (most relevant for longer-form segmentation)
- `HarmonicPercussive Source Separation (HPSS)` — splits audio into harmonic and percussive streams

**Decomposition:**
- `NMF` — Non-negative Matrix Factorization; factorizes a spectrogram into latent components (useful for finding repeating spectral "themes" in a corpus)
- `Sines / Residual / Transients` — sinusoidal modeling to separate pitched, noisy, and transient content

### The ML Pipeline

Once segments are extracted and described, FluCoMa applies:

1. **Normalization / Standardization / Robust Scaler** — brings all descriptors to comparable scale
2. **Dimensionality Reduction:**
   - PCA (Principal Component Analysis) — linear reduction, preserves global variance
   - UMAP (Uniform Manifold Approximation and Projection) — non-linear reduction, preserves local topology; dramatically better for audio corpora than PCA
   - MDS (Multidimensional Scaling) — distance-preserving reduction
3. **Clustering:** KMeans / SKMeans — groups segments into perceptual families
4. **KDTree** — fast nearest-neighbor lookup in feature space (enables real-time "find the most similar segment to this one")

The result is the **toposonic map** — a 2D coordinate space where proximity means perceptual similarity. Vogel's key insight: make this map the performance surface itself.

---

## 4. VOGEL'S COMPOSITIONAL PHILOSOPHY

Several threads emerge across interviews, blog posts, and the design of his instruments:

### 4.1 Spectral Thinking as Base Layer

*"Cristian's understanding of music now is spectral. With every step through his exploration of sound, he has made more and more detailed analyses of the specific frequencies that make up specific sounds."*

He does not think in notes or rhythms as primary material. He thinks in **spectral envelopes, timbral trajectories, and frequency relationships over time**. This maps directly onto MIR descriptors — SpectralShape and MFCCs are mathematical expressions of exactly this.

### 4.2 The Algorithm as Composer

*"I'm not in need of advanced music theory — the computers I work with are doing a better job of figuring out what notes are coming next than I probably could."*

He designs systems where **musical decisions are delegated to the analysis layer**. The human performer navigates a space that the machine has organized. This is not AI generation — it is curated machine intelligence shaping a performance surface.

### 4.3 Micro Sound as Philosophy

From his GrainFrame documentation: *"Working at the extremities of perceptual time scales, we find... the construction of larger sounds by manipulating many small fragments of audio can lead to new timbral vocabularies."*

He draws explicitly on **Curtis Roads' Microsound** tradition — the idea that the grain (5–500ms fragment) is the fundamental unit of sonic composition, not the note. His entire granular practice operates at this level.

### 4.4 Small Data is Beautiful Data

Influenced by Rebecca Fiebrink's work: you do not need a massive dataset to make intelligent musical decisions. **A single carefully analyzed audio file** contains enough variation to build an entire performance system from. This is the principle behind the corpus instrument concept.

### 4.5 Kyma's Legacy in Incline

His 15+ years building in Kyma gave Vogel deep fluency in **signal-domain thinking** — constructing audio not from pre-baked modules but from fundamental signal flow graphs. Incline inherits this: the analysis pipeline is not a black box plugin but a configured signal chain where every parameter of segmentation and description is exposed and performable.

---

## 5. THE NOVELTY SLICE — THE KEY ALGORITHM FOR SEISMIC

This is the FluCoMa algorithm most directly relevant to SOS/SEISCLAW, and worth understanding deeply.

### How It Works

1. Transform the audio into the spectral domain via STFT (Short-Time Fourier Transform) — producing a sequence of spectral frames
2. Compute a **Self-Similarity Matrix (SSM)** — a grid where each cell (i, j) represents how similar frame i is to frame j, using Euclidean distance between spectral magnitudes
3. Compute a **novelty curve** — for each moment, sum the differences in a window around that point in the SSM. High novelty = that moment is spectrally different from its recent past
4. Find **peaks in the novelty curve** — these are the segment boundaries
5. The `kernel size` parameter controls how long a window is considered — larger kernels produce longer slices (structural segmentation), smaller kernels produce fine-grained event detection

### Why This Matters for Seismic

Seismic waveforms are **non-stationary, complex, and often lack the sharp transient onsets** that amplitude-based or spectral-flux onset detectors rely on. But they are *structurally heterogeneous* — P-waves, S-waves, surface waves, codas, and ambient tremor are all spectrally distinct from each other.

**NoveltySlice is the natural segmentation algorithm for seismic data** because:
- It detects structural change rather than amplitude change
- It handles gradual, emergent events (like teleseismic arrivals that build slowly)
- The kernel size maps directly to seismic time scales — small for local microseismic events, large for structural segmentation of a teleseism
- It works in the absence of sharp attacks (most seismic energy is emergent, not impulsive)

---

## 6. DIRECT APPLICATION MAP: VOGEL'S APPROACH → SOS / SEISCLAW

The following maps each component of Vogel's Incline architecture onto a proposed SOS/SEISCLAW equivalent.

### 6.1 The Corpus

| Incline | SOS/SEISCLAW |
|---|---|
| Fixed audio file (WAV/AIFF) | Rolling MiniSEED buffer (e.g., 30–300 seconds of live waveform) |
| Static corpus, analyzed offline | Live corpus, continuously re-analyzed as new seismic data arrives |
| User loads arbitrary audio | Earth generates the corpus autonomously |

**Key difference and opportunity**: Incline works offline on a fixed corpus. SEISCLAW would be the **first implementation of this paradigm on a live, continuously updating corpus** — the corpus itself is alive and evolving.

### 6.2 Segmentation Layer

| Incline | SOS/SEISCLAW |
|---|---|
| AmplitudeSlice for percussive events | Map to local microseismic events / teleseismic P-wave arrivals |
| OnsetSlice for timbral changes | Map to wave phase transitions (P→S→Surface) |
| NoveltySlice for structural sections | Map to earthquake vs. ambient vs. tremor segmentation |
| TransientSlice for attack isolation | Map to impulsive seismic sources (explosions, quarry blasts, ice quakes) |

**Implementation note**: In the Web Audio API context of SOS, these algorithms can be approximated using the existing FFT infrastructure. The full FluCoMa C++ implementation is not required — the *concepts* translate directly to JavaScript equivalents.

### 6.3 Descriptor Extraction → Synthesis Mapping

This is the core of the Vogel method: **each segment is described numerically, and those numbers drive synthesis parameters**.

| FluCoMa Descriptor | Seismic Interpretation | Synthesis Target in SEISCLAW |
|---|---|---|
| `Loudness (LUFS)` | Seismic amplitude / magnitude proxy | Grain density, output level |
| `SpectralCentroid` | Dominant frequency content (high = high-freq P-wave energy; low = surface wave energy) | Granular playback rate, filter cutoff |
| `SpectralFlatness` | Noise vs. tonal character (ambient noise = flat; harmonic tremor = tonal) | Oscillator vs. noise synthesis ratio |
| `SpectralSpread` | Bandwidth of energy distribution | Grain size scatter, filter bandwidth |
| `MFCCs` | Full timbral fingerprint of the wave phase | Synthesis engine selection (which synth "voice" plays this grain) |
| `Pitch + Confidence` | Presence of harmonic tremor / periodic signals | Pitch-tracking oscillator activation |
| `NoveltyScore` | Rate of structural change | Grain spawn rate, tempo/density modulation |

### 6.4 Dimensionality Reduction → Performance Space

Vogel's toposonic map concept applied to SEISCLAW:

- Extract 5–13 descriptors per seismic segment
- Apply UMAP to reduce to 2D
- The resulting 2D space is a **seismological feature map** where:
  - Clusters represent recurring seismic event types (ambient, teleseismic, local, tremor)
  - Distance represents perceptual/structural dissimilarity
  - Proximity represents seismic similarity
- Traverse this map over time — the Earth's seismic activity creates a **temporal path through the feature space**, and that path is the composition

This is a profound conceptual alignment: **the Earth is not just generating audio, it is navigating its own corpus**.

### 6.5 Granular Modes → SEISCLAW Agent Architecture

| Incline Mode | SEISCLAW Agent |
|---|---|
| One Shot | Event Agent — triggers single-grain playback on detected seismic events |
| One Loop | Texture Agent — continuously traverses the feature space near current seismic state, creating sustained evolving textures |
| Cloud | Swarm Agent — distributes grains across the feature space weighted by seismic similarity, producing dense electronica clouds |
| Contour Sequencer | Autonomous Conductor — follows a path through the feature space determined by seismic trajectory over hours/days |

---

## 7. THE NOVELTY CURVE AS SEISCLAW'S COMPOSITIONAL ENGINE

Here is the most radical application: rather than using seismic amplitude to drive synthesis parameters (the current SOS approach), use the **novelty curve of the seismic waveform as the primary compositional driver**.

### Practical Implementation

```
1. Buffer 60 seconds of live MiniSEED waveform data
2. Compute STFT (e.g., 1024-point FFT, 512-point hop)
3. Build a self-similarity matrix from spectral frames
4. Compute novelty curve (kernel size = 8–16 frames = ~0.5–1 second)
5. Find peaks in novelty curve → these are "seismic events" regardless of amplitude
6. For each detected event:
   - Extract descriptors (centroid, flatness, loudness, spread)
   - Map descriptors to synthesis parameters
   - Trigger grain / note / texture based on event type classification
```

The result: **the seismograph becomes a score**, and the score is read by MIR analysis rather than by amplitude threshold.

### Why This Is Superior to the Current Approach

Current SOS: seismic amplitude → synthesis parameter (direct linear mapping)

Proposed: seismic structure → feature extraction → dimensionality reduction → synthesis parameter (intelligent, musically meaningful mapping)

The difference: the current approach makes the *loudness* of the Earth drive the music. The proposed approach makes the *character* of the Earth's motion drive the music. A distant magnitude 6.0 earthquake and a nearby magnitude 2.0 earthquake may have similar amplitudes at the sensor but completely different spectral characters. MIR distinguishes them. Direct amplitude mapping cannot.

---

## 8. IMPLEMENTATION PATHWAY FOR SOS/SEISCLAW

### Phase 1: JavaScript MIR Primitives (Web Audio API)

Implement the core descriptor extraction natively in the existing SOS architecture:

- **SpectralCentroid**: already computable from the FFT analyser node
- **SpectralFlatness**: geometric mean / arithmetic mean of FFT bins
- **SpectralSpread**: variance around the centroid
- **RMS Loudness**: from time-domain samples
- **Zero Crossing Rate**: simple temporal descriptor, useful for noise vs. tonal detection

These can be computed per-frame (every 1024 samples = ~23ms at 44.1kHz) and fed directly into synthesis parameter mappings.

### Phase 2: Novelty-Based Segmentation

Build a rolling SSM using a circular buffer of recent spectral frames. Compute novelty scores in real time. Use novelty peaks to gate grain generation — grains only spawn when there is genuine structural change in the seismic signal.

### Phase 3: Feature Space Navigation

Maintain a 2D running history of (SpectralCentroid, SpectralFlatness) or similar reduced feature pairs. This creates a live "seismological terrain" that can be visualized (extending the existing Three.js shadow zone work) and traversed by the synthesis engine.

### Phase 4: Event Classification

Use a simple KMeans-like classifier (3–5 clusters) trained on the live descriptor stream to distinguish between seismic regimes: ambient noise, teleseismic arrivals, local events, tremor, and anthropogenic noise. Each class drives a different synthesis voice — analogous to Vogel's multi-mode architecture.

---

## 9. CONCEPTUAL ALIGNMENT: VOGEL'S PHILOSOPHY AND SEISCLAW'S MISSION

The deepest resonance between Vogel's practice and SEISCLAW is philosophical:

**Vogel**: the corpus is treated as an already-composed piece of music. The instrument's job is to reveal that composition by navigating the feature space intelligently.

**SEISCLAW**: the Earth's seismic activity is treated as an already-composed piece of music. The system's job is to reveal that composition by interpreting the waveform intelligently.

Both are fundamentally **anti-generative** in the conventional AI sense — they are not making things up. They are *listening carefully* and *translating faithfully*. The intelligence is in the translation layer, not the output.

Vogel's term **"toposonic"** — treating sound as terrain — maps uncannily well onto seismology, which literally studies terrain through sound. The feature space that Incline navigates for a Kyma recording is structurally identical to the feature space that SEISCLAW could navigate for a live seismogram. The Earth is its own corpus. It has already composed everything. The system's job is to explore it.

---

## 10. KEY REFERENCES & RESOURCES

- **NeverEngineLabs / Incline**: neverenginelabs.com/products/incline-toposonic-corpus-explorer
- **FluCoMa project**: flucoma.org / learn.flucoma.org
- **FluCoMa C++ core** (for Rust FFI integration): github.com/flucoma/flucoma-core
- **NoveltySlice algorithm paper**: Foote, J. (2000). Automatic audio segmentation using a measure of audio novelty. ICME. ccrma.stanford.edu/workshops/mir2009/references/Foote_00.pdf
- **Cristian Vogel / MusicRadar interview**: musicradar.com (February 2021) — key statements on spectral thinking and algorithmic composition
- **Cristian Vogel / Bitwig interview**: bitwig.com/artists/cristian-vogel — on wavetables, Xenakis, and timbral morphing
- **GrainFrame documentation**: medium.com/@neverenginelabs — Vogel's theoretical framework for micro sound composition
- **Curtis Roads — Microsound** (MIT Press, 2001) — foundational text for all granular synthesis theory
- **FluCoMa CCRMA Workshop syllabus**: ccrma.stanford.edu/workshops/flucoma — complete curriculum for MFCCs, UMAP, clustering, concatenative synthesis

---

*Research compiled March 2026. Primary sources: NeverEngineLabs product documentation, FluCoMa learning resources, Cristian Vogel interviews (MusicRadar, Bitwig, Medium/NeverEngineLabs), FluCoMa discourse archives, and seismic signal processing literature.*
