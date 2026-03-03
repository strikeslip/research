# MLX on Mac Mini M4 (24GB RAM) + Best Qwen Models Guide

**Document Version:** March 3, 2026  
**Hardware Focus:** Apple Mac Mini M4 (base or Pro variant) with 24GB unified memory  
**Framework:** Apple's MLX (https://github.com/ml-explore/mlx) + mlx-lm for efficient local LLM inference on Apple Silicon  
**Primary Topic:** Running the latest Qwen models (especially Qwen3.5 series) locally — performance, memory fit, speed estimates, recommendations

## 1. Why MLX + Mac Mini M4 24GB Is Excellent for Local LLMs

MLX is an open-source array framework optimized for Apple Silicon (M1–M5 chips). It leverages:
- **Unified memory** — No slow CPU↔GPU data copies; everything shares the same fast RAM pool.
- **Metal GPU acceleration** — High bandwidth on M4 (~120–150 GB/s).
- **Lazy computation & dynamic graphs** — Efficient for experimentation.
- **mlx-lm** package — One-command inference/fine-tuning for thousands of Hugging Face models (especially quantized ones from **mlx-community**).

**Mac Mini M4 24GB specifics (early 2026 real-user reports):**
- Runs 7B–14B dense models at 40–70+ tokens/sec (t/s) quantized 4-bit.
- Handles up to ~27B–35B MoE (low active params) quantized if careful with context.
- ~5–12GB headroom typical after OS + model load → great for 8k–32k context, multitasking (browser, IDE).
- Snappy time-to-first-token; low power/heat compared to discrete GPUs.

## 2. Qwen3.5 Series Overview (Latest as of March 3, 2026)

Alibaba's Qwen team released **Qwen3.5** starting February 16, 2026 (flagship 397B-A17B MoE), with medium sizes ~Feb 24, and the **Small Series** on **March 2, 2026**:

- **Small Series** (dense, on-device focused, native multimodal: text + image + video):
  - Qwen3.5-0.8B
  - Qwen3.5-2B
  - Qwen3.5-4B
  - Qwen3.5-9B
- **Medium/Large** (MoE hybrids for efficiency + frontier performance):
  - Qwen3.5-27B (dense)
  - Qwen3.5-35B-A3B (~3B active)
  - Qwen3.5-122B-A10B (~10B active)
  - Qwen3.5-397B-A17B (~17B active flagship)

Key features across series:
- 201+ languages/dialects
- 262k native context (up to 1M+ extended on some)
- Strong reasoning, coding, agentic capabilities
- Multimodal native (vision-language from training)
- Apache 2.0 license → fully FOSS
- Available: Hugging Face (Qwen org), ModelScope

**Small series highlights (March 2 release):**
- Designed for laptops/phones/edge
- Qwen3.5-9B often beats much larger prior models (e.g., Qwen3-30B equivalents, even some 120B rivals) on MMLU-Pro, multilingual, graduate reasoning, vision benchmarks

## 3. Best Qwen Models for Mac Mini M4 24GB + MLX (March 2026)

Prioritize quantized versions from **mlx-community** on Hugging Face (4-bit/5-bit/6-bit/8-bit common; bf16 for max quality if fits).

| Model                          | Params     | Quant Level | Est. Memory Use | Est. Speed (t/s) | Strengths / Best For                  | Fit on 24GB?     |
|--------------------------------|------------|-------------|-----------------|------------------|----------------------------------------|------------------|
| **Qwen3.5-9B-Instruct**       | 9–10B     | 4-bit      | ~5–7GB         | 50–70+          | Balanced reasoning/coding/multimodal; punches above weight | Excellent (top pick) |
| Qwen3.5-9B-Instruct           | 9–10B     | 5-bit/6-bit| ~6–8GB         | 45–65           | Slightly higher quality vs 4-bit      | Excellent       |
| Qwen3.5-4B-Instruct           | 4–5B      | 4-bit      | ~3–5GB         | 70–100+         | Ultra-fast, still capable reasoning   | Excellent (speed demon) |
| Qwen3.5-2B / 0.8B             | 2B / 0.8B | 4-bit      | ~1.5–3GB       | 90–120+         | Instant responses, edge-like efficiency | Excellent       |
| Qwen3.5-27B (if quantized)    | 27B       | 4-bit      | ~15–18GB       | 20–35           | Stronger raw capability; marginal buffer | Tight (possible, short context) |
| Qwen3.5-35B-A3B MoE           | 35B total / 3B active | 4–6-bit | ~18–25GB+     | 30–50           | Frontier MoE efficiency; agentic strong | Marginal/tight (swapping risk) |

**Current #1 Recommendation:** **Qwen3.5-9B-Instruct-4bit** (or 5-bit) via mlx-community  
- Fits with huge headroom  
- Fast & smart (beats older 30B models on many evals)  
- Multimodal bonus (describe images/code screenshots)  
- Fresh MLX quants uploaded ~March 3 (within hours of release)

Larger MoE like 35B-A3B run on higher-spec M4 Pro (64GB) at ~50–60 t/s but push 24GB limits.

## 4. Installation & Quick Start

```bash
# Install / update
pip install --upgrade mlx mlx-lm

# Run example (replace with exact mlx-community repo name)
python -m mlx_lm.generate \
  --model mlx-community/Qwen3.5-9B-Instruct-4bit \
  --prompt "Write a Python CLI tool that..." \
  --max-tokens 1024 \
  --temp 0.7
```

Tips:
- Use `--max-kv-size` or limit context for bigger models.
- Try LM Studio (excellent MLX backend + UI) or Ollama (if MLX support added).
- Check https://huggingface.co/mlx-community for newest quants (sort by recent).

## 5. Performance Expectations & Tips

- **Coding:** Qwen3.5-9B strong on quick scripts/prototypes; approaches Sonnet-level on medium tasks but lags frontier (Opus 4.6 / Sonnet 4.6) on complex multi-file/agentic repos.
- **Multimodal:** Native vision → feed diagrams/screenshots for code explanation.
- **Future Outlook:** Chinese FOSS (Qwen/GLM/DeepSeek/MiniMax) iterating fast — expect near-Opus local parity on 24GB hardware by late 2026/2027 via better MoE + quants.

## 6. Resources

- MLX GitHub: https://github.com/ml-explore/mlx  
- mlx-lm: https://github.com/ml-explore/mlx-lm  
- Qwen Hub: https://huggingface.co/Qwen  
- MLX Community Quants: https://huggingface.co/mlx-community  
- Official Blog: https://qwen.ai/blog  

**Happy local inferencing on your M4 Mini!** 🚀  
(Scene moves fast — re-check mlx-community daily for new Qwen3.5 drops.)

---------------

# MLX on Mac Mini M4 (24GB RAM) + Best Qwen Models Guide  
**For SeisClaw & Autonomous SOS Agents** (March 2026)

**Document Version:** March 3, 2026  
**Hardware:** Apple Mac Mini M4 (base/Pro) with **24GB unified memory**  
**Framework:** Apple’s MLX + mlx-lm  
**Project Context:** This guide is tailored for **seisclaw.com** (real-time seismic sonification art) and the broader **Sounds of Seismic (SOS)** ecosystem (sos.allshookup.org).  
The goal: build powerful agents now with Claude Sonnet/Opus 4.6, then migrate them to run **fully autonomously and locally** on your M4 Mini using open-source FOSS models like **Qwen3.5** (and future versions).

---

## 1. Why MLX + Mac Mini M4 24GB Is Perfect for SeisClaw Agents

MLX is purpose-built for Apple Silicon:
- Unified memory → zero-copy CPU/GPU sharing (ideal for real-time audio synthesis + data processing)
- Metal acceleration → high bandwidth on M4
- mlx-lm → one-command inference for quantized models

**Your 24GB Mac Mini strengths:**
- Runs 9B–14B models at **50–70+ tokens/sec**
- Comfortable headroom for 128k+ context + background audio processing
- Always-on, low-power, silent → perfect dedicated “seismic brain” machine

This setup is already strong enough for **local autonomous agents** that keep SOS alive 24/7.

---

## 2. Qwen3.5 Series – The Latest (as of March 3, 2026)

Alibaba’s newest open-weight family (Apache 2.0):

**Small Series** (released March 2, 2026 – ideal for your hardware):
- Qwen3.5-0.8B / 2B / 4B / **9B** (dense, native multimodal)

**Medium/Large MoE** (for future scaling):
- Qwen3.5-27B, 35B-A3B (~3B active), 122B-A10B, 397B-A17B

**Why Qwen3.5 matters for SeisClaw:**
- Excellent coding + agentic reasoning
- Native vision (feed spectrograms or seismic plots directly)
- 262k native context (perfect for long seismic event histories)
- Extremely efficient quantization

---

## 3. Best Qwen Models for Your M4 Mini 24GB Right Now

All quantized via **mlx-community** on Hugging Face:

| Model                        | Params   | Quant | Memory Use | Speed (t/s) | Best SeisClaw Use Case                     | Fit     |
|-----------------------------|----------|-------|------------|-------------|--------------------------------------------|---------|
| **Qwen3.5-9B-Instruct**    | 9–10B   | 4-bit | ~5–7GB    | 50–70+     | Main agent brain (sonification logic, USGS polling, GitHub commits) | **Top pick** |
| Qwen3.5-9B-Instruct        | 9–10B   | 5-bit | ~6–8GB    | 45–65      | Higher-quality creative decisions          | Excellent |
| Qwen3.5-4B-Instruct        | 4–5B    | 4-bit | ~3–5GB    | 70–100+    | Fast helper agents (real-time audio tweaks) | Excellent |
| Qwen3.5-27B (quantized)    | 27B     | 4-bit | ~15–18GB  | 20–35      | Advanced multi-step agents (future)        | Tight but doable |
| Qwen3.5-35B-A3B MoE        | 35B total | 4–6-bit | ~18–25GB | 30–50    | Frontier agentic coding (late 2026)        | Marginal (upgrade path) |

**Current recommendation:** Start with **Qwen3.5-9B-Instruct-4bit** — it already handles most agent tasks you’re prototyping with Claude today.

---

## 4. The SeisClaw Agent Migration Path

**Phase 1 – Now (March 2026)**  
Build and test agents using **Claude Sonnet/Opus 4.6** (cloud) for maximum reliability:
- Real-time USGS/iris.edu quake polling
- Waveform → granular synthesis → electronica generation
- Auto-update seisclaw.com + GitHub + SOS posts

**Phase 2 – Late 2026 / Early 2027**  
When Qwen (or GLM/DeepSeek equivalents) reaches near-Opus 4.6 agentic performance:
- Port the exact same agent code to **local MLX inference**
- Run fully offline, 24/7 on your M4 Mini
- Multi-agent swarm example:
  - **Monitor Agent** → watches seismic feeds
  - **Sonify Agent** → decides on interesting events and generates new tracks
  - **Creative Agent** → creates variations, artwork, social posts
  - **Deploy Agent** → commits to GitHub, updates site, restarts on crash

**Result:** SOS stays alive autonomously — new seismic electronica generated and published even while you sleep. No API bills, no rate limits, total creative control.

---

## 5. Installation & Quick Start (SeisClaw Ready)

```bash
pip install --upgrade mlx mlx-lm

# Test your future agent brain
python -m mlx_lm.generate \
  --model mlx-community/Qwen3.5-9B-Instruct-4bit \
  --prompt "Write a Python agent that polls USGS every 5 minutes, sonifies new quakes using granular synthesis, and commits new audio to GitHub" \
  --max-tokens 2048
