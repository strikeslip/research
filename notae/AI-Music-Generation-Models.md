# AI Music Generation Models — Public Directory

**Source:** [Music Arena Beta](https://beta.music-arena.org/) Leaderboard Snapshot, February 27, 2026

---

## 1. Riffusion FUZZ (Producer.ai)

**Arena Rank:** #1 (fuzz-1-0) & #2 (fuzz-1-1)  
**Type:** Proprietary (Google Deepmind)<br>
**Organization:** Producer.ai (formerly Riffusion)

| Link | URL |
|------|-----|
| Producer.ai (main site) | https://www.producer.ai/ |
| Classic Riffusion | https://classic.riffusion.com/ |
| GitHub (original open-source) | https://github.com/riffusion/riffusion |
| X / Twitter | https://x.com/producer_ai |
| Community repo (FUZZ-2.0 prompts) | https://github.com/rchmnnr/fuzz2-producer-ai-angklungmann |

**Notes:** Riffusion rebranded to Producer.ai in mid-2025. The FUZZ model family uses spectrogram-based diffusion to generate full songs from text, audio, and visual prompts. Training data is unspecified.

---

## 2. Sonauto

**Arena Rank:** #3 (sonauto-v2-2)  
**Type:** Proprietary  
**Organization:** Sonauto (YC-backed)

| Link | URL |
|------|-----|
| Sonauto (main site) | https://sonauto.ai/ |
| fal.ai API (text-to-music) | https://fal.ai/models/sonauto/v2/text-to-music |
| fal.ai API (extend/audio-to-audio) | https://fal.ai/models/sonauto/v2/extend/api |
| Y Combinator profile | https://www.ycombinator.com/companies/sonauto |
| Tag Explorer | https://sonauto.ai/tag-explorer |

**Notes:** Built on a latent diffusion model (Melodia). Generates full songs up to 4.5 minutes with vocals. Unlimited free tier. Training data unspecified.

---

## 3. ElevenLabs Music v1

**Arena Rank:** #4 (elevenlabs-music-v1)  
**Type:** Proprietary  
**Organization:** ElevenLabs

| Link | URL |
|------|-----|
| ElevenLabs (main site) | https://elevenlabs.io/ |
| Music overview docs | https://elevenlabs.io/docs/overview/capabilities/music |
| Music best practices | https://elevenlabs.io/docs/overview/capabilities/music/best-practices |
| API reference (compose) | https://elevenlabs.io/docs/api-reference/music/compose |
| Music v1 terms | https://elevenlabs.io/eleven-music-v1-terms |
| Models overview | https://elevenlabs.io/docs/overview/models |

**Notes:** Studio-grade text-to-music model. Created in collaboration with labels, publishers, and artists — the only top model explicitly trained on licensed data. Supports composition plans, section editing, and multilingual lyrics. Up to 5 minutes duration.

---

## 4. Google DeepMind — Magenta RealTime

**Arena Rank:** #5 (magenta-rt-large)  
**Type:** Open weights  
**Organization:** Google DeepMind (Magenta Project)

| Link | URL |
|------|-----|
| Magenta RT blog post | https://magenta.withgoogle.com/magenta-realtime |
| Hugging Face (weights) | https://huggingface.co/google/magenta-realtime |
| Magenta Project (main) | https://magenta.tensorflow.org/ |
| GitHub (Magenta legacy) | https://github.com/magenta/magenta |
| Lyria RealTime API | https://magenta.withgoogle.com/lyria-realtime |
| Lyria 3 (proprietary cousin) | https://deepmind.google/models/lyria/ |
| Technical report (arXiv) | https://arxiv.org/abs/2506.17926 |

**Notes:** 800M parameter autoregressive transformer. Open weights (Apache 2.0 code / CC-BY 4.0 weights). Trained on ~190k hours of stock instrumental music. Near real-time generation (1.01 RTF). Designed for live performance and interactive co-creation, not finished song generation.

---

## 5. Stability AI — Stable Audio Open (SAO)

**Arena Rank:** #6 (sao) & #9 (sao-small)  
**Type:** Open weights / Open data  
**Organization:** Stability AI

| Link | URL |
|------|-----|
| Stable Audio (product site) | https://www.stableaudio.com/ |
| Stability AI audio page | https://stability.ai/stable-audio |
| Hugging Face (SAO 1.0 weights) | https://huggingface.co/stabilityai/stable-audio-open-1.0 |
| GitHub (stable-audio-tools) | https://github.com/Stability-AI/stable-audio-tools |
| Research paper (arXiv) | https://arxiv.org/abs/2407.14358 |
| Replicate (Stable Audio 2.5) | https://replicate.com/stability-ai/stable-audio-2.5 |

**Notes:** Transformer-based diffusion model (DiT). Open weights trained entirely on CC-licensed data from Freesound and Free Music Archive (~486k recordings). Generates up to 47s stereo audio at 44.1kHz. SAO-small (341M params) is optimized for mobile/on-device.

---

## 6. Meta — MusicGen

**Arena Rank:** #7 (musicgen-medium) & #8 (musicgen-small)  
**Type:** Open weights  
**Organization:** Meta FAIR

| Link | URL |
|------|-----|
| Hugging Face (medium) | https://huggingface.co/facebook/musicgen-medium |
| Hugging Face (small) | https://huggingface.co/facebook/musicgen-small |
| Hugging Face (large) | https://huggingface.co/facebook/musicgen-large |
| Hugging Face (stereo-medium) | https://huggingface.co/facebook/musicgen-stereo-medium |
| Hugging Face (melody variant) | https://huggingface.co/facebook/musicgen-melody |
| Hugging Face demo space | https://huggingface.co/spaces/facebook/MusicGen |
| GitHub (AudioCraft) | https://github.com/facebookresearch/audiocraft |
| Paper | https://arxiv.org/abs/2306.05284 |
| Transformers docs | https://huggingface.co/docs/transformers/en/model_doc/musicgen |

**Notes:** Single-stage autoregressive transformer over EnCodec tokens. Trained on licensed stock music (Meta Music Initiative, Shutterstock, Pond5). Fastest generation in the arena at 0.45 RTF (medium). Up to 30 seconds per generation. Melody conditioning available.

---

## Arena Platform

| Link | URL |
|------|-----|
| Music Arena (CMU / gclef) | https://music-arena.org/ |
| Music Arena Beta | https://beta.music-arena.org/ |
| Artificial Analysis Music Arena | https://artificialanalysis.ai/music/arena?tab=leaderboard |
| Hugging Face leaderboard space | https://huggingface.co/spaces/ArtificialAnalysis/Music-Arena-Leaderboard |
| Arena paper (arXiv) | https://arxiv.org/abs/2507.20900 |
| GitHub (arena source) | https://github.com/gclef-cmu/music-arena |

---

*Compiled February 27, 2026*
