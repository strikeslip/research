# The Top 26 Essential LLM Papers + 5 Bonus Resources
## + RTNeural Addendum

---

## 1. Attention Is All You Need (Vaswani et al., 2017)
**URL:** https://arxiv.org/abs/1706.03762

The foundational Transformer paper that replaced RNNs with self-attention mechanisms. Introduced multi-head attention, positional encoding, and the encoder-decoder architecture that powers all modern LLMs.

---

## 2. The Illustrated Transformer (Jay Alammar, 2018)
**URL:** https://jalammar.github.io/illustrated-transformer/

Visual guide to understanding Transformer internals. Essential intuition builder for attention mechanisms, query-key-value operations, and tensor flow before diving into technical implementations.

---

## 3. BERT: Pre-training of Deep Bidirectional Transformers (Devlin et al., 2018)
**URL:** https://arxiv.org/abs/1810.04805

Introduced bidirectional training via masked language modeling. Established encoder-side representation learning that still influences modern architecture design and downstream task performance.

---

## 4. Language Models are Few-Shot Learners / GPT-3 (Brown et al., 2020)
**URL:** https://arxiv.org/abs/2005.14165

Demonstrated in-context learning at scale with 175B parameters. Shifted understanding of how prompting works and proved LLMs could perform tasks without fine-tuning through few-shot examples.

---

## 5. Scaling Laws for Neural Language Models (Kaplan et al., 2020)
**URL:** https://arxiv.org/abs/2001.08361

First empirical framework relating model performance to parameters, dataset size, and compute. Established power-law relationships but underestimated optimal token counts (corrected by Chinchilla).

---

## 6. Training Compute-Optimal Large Language Models / Chinchilla (Hoffmann et al., 2022)
**URL:** https://arxiv.org/abs/2203.15556

Corrected Kaplan's scaling laws to show most LLMs were undertrained relative to their size. Established the ~20 tokens per parameter rule and reshaped how organizations budget training runs.

---

## 7. Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks (Lewis et al., 2020)
**URL:** https://arxiv.org/abs/2005.12365

Introduced RAG as a framework for grounding LLMs in external knowledge bases. Foundation for most production retrieval systems combining dense passage retrieval with generative models.

---

## 8. Constitutional AI: Harmlessness from AI Feedback (Bai et al., 2022)
**URL:** https://arxiv.org/abs/2212.08073

Anthropic's framework for training AI systems to be helpful, harmless, and honest using AI-generated feedback rather than human labeling. Basis for RLHF refinement techniques.

---

## 9. InstructGPT: Training Language Models to Follow Instructions (Ouyang et al., 2022)
**URL:** https://arxiv.org/abs/2203.02155

Demonstrated that RLHF on small curated datasets dramatically improves instruction-following. Showed alignment improvements don't require massive compute — the alignment tax can be small.

---

## 10. LLaMA: Open and Efficient Foundation Language Models (Touvron et al., 2023)
**URL:** https://arxiv.org/abs/2302.13971

Meta's open foundation models. Proved competitive performance at smaller scales and democratized LLM research by releasing weights openly. Spawned the open-source LLM ecosystem.

---

## 11. Sparks of Artificial General Intelligence: Early Experiments with GPT-4 (Bubeck et al., 2023)
**URL:** https://arxiv.org/abs/2303.12528

Microsoft Research's extensive evaluation of GPT-4 across domains. Argued early signs of general intelligence while establishing important capability benchmarks and limitations.

---

## 12. Direct Preference Optimization: Your Language Model is Secretly a Reward Model (Rafailov et al., 2023)
**URL:** https://arxiv.org/abs/2305.18290

Simplified RLHF by directly optimizing preferences through a stable classification loss. Removed the need for a separate reward model, making alignment training more accessible.

---

## 13. Chain-of-Thought Prompting Elicits Reasoning in Large Language Models (Wei et al., 2022)
**URL:** https://arxiv.org/abs/2201.11903

Showed that prompting models to show intermediate reasoning steps dramatically improves performance on complex tasks. Laid groundwork for reasoning-focused training approaches.

---

## 14. ReAct: Synergizing Reasoning and Acting in Language Models (Yao et al., 2022)
**URL:** https://arxiv.org/abs/2210.03629

Foundation of agentic AI systems. Interleaves reasoning traces with actions, enabling tool use and environment interaction through structured thought-action-observation loops.

---

## 15. DeepSeek-R1: Incentivizing Reasoning Capability in LLMs via Reinforcement Learning (Guo et al., 2025)
**URL:** https://arxiv.org/abs/2501.12948

Proved large-scale RL without supervised data can induce self-verification and structured reasoning. Models learn to check their own work through pure reinforcement learning signals.

---

## 16. Qwen2.5 Technical Report (Yang et al., 2024)
**URL:** https://arxiv.org/abs/2412.15115

Modern architecture overview with unified MoE design. Introduced Thinking Mode and Non-Thinking Mode to dynamically trade off computational cost against reasoning depth.

---

## 17. Outrageously Large Neural Networks: The Sparsely-Gated Mixture-of-Experts Layer (Shazeer et al., 2017)
**URL:** https://arxiv.org/abs/1701.06538

Reignited Mixture of Experts for modern deep learning. Demonstrated conditional computation at scale, activating only a subset of parameters per token.

---

## 18. Switch Transformers: Scaling to Trillion Parameter Models (Fedus et al., 2021)
**URL:** https://arxiv.org/abs/2101.03961

Simplified MoE routing to single-expert activation per token. Achieved trillion-parameter training with better stability and enabled massive model scaling.

---

## 19. Mixtral of Experts (Mistral AI, 2024)
**URL:** https://arxiv.org/abs/2401.04088

Open-weight sparse MoE proving sparse models can match dense quality while running at small-model inference cost. Democratized efficient large-scale models.

---

## 20. Sparse Upcycling: Training Mixture-of-Experts from Dense Checkpoints (Komatsuzaki et al., 2022)
**URL:** https://arxiv.org/abs/2212.05055

Practical technique for converting dense checkpoints into MoE models without training from scratch. Critical for compute reuse and iterative scaling experiments.

---

## 21. The Platonic Representation Hypothesis (Huh et al., 2024)
**URL:** https://arxiv.org/abs/2405.07987

Evidence that models trained on different modalities and tasks converge toward shared internal representations at scale. Suggests universal structure in learned representations.

---

## 22. Textbooks Are All You Need (Gunasekar et al., 2023)
**URL:** https://arxiv.org/abs/2306.11644

Demonstrated high-quality synthetic data allows small models to outperform much larger ones. Established data quality as a critical scaling variable alongside raw volume.

---

## 23. LoRA: Low-Rank Adaptation of Large Language Models (Hu et al., 2021)
**URL:** https://arxiv.org/abs/2106.09685

Made fine-tuning of large models tractable by injecting trainable low-rank matrices. Enabled task adaptation with a fraction of full fine-tuning cost. Now standard in production workflows.

---

## 24. FlashAttention: Fast and Memory-Efficient Exact Attention (Dao et al., 2022)
**URL:** https://arxiv.org/abs/2205.14135

Solved the memory bottleneck of standard attention via IO-aware tiling. Enabled training of long-context models and became a near-universal component of modern training stacks.

---

## 25. Mamba: Linear-Time Sequence Modeling with Selective State Spaces (Gu & Dao, 2023)
**URL:** https://arxiv.org/abs/2312.00752

Proposed selective state space models as a Transformer alternative with linear scaling. Demonstrated competitive performance on language with significantly better long-sequence efficiency.

---

## 26. GPT-4 Technical Report (OpenAI, 2023)
**URL:** https://arxiv.org/abs/2303.08774

OpenAI's overview of GPT-4 capabilities, training methodology, and safety evaluations. Established multimodal LLM benchmarks and documented the RLHF and red-teaming pipeline.

---

## Bonus Resources

- **The Illustrated GPT-2** — https://jalammar.github.io/illustrated-gpt2/
- **Andrej Karpathy: Let's Build GPT** — https://www.youtube.com/watch?v=kCc8FmEb1nY
- **Lilian Weng: Attention? Attention!** — https://lilianweng.github.io/posts/2018-06-24-attention/
- **Lilian Weng: Large Language Model** — https://lilianweng.github.io/posts/2023-01-27-the-transformer-family-v2/
- **Hugging Face Transformers Docs** — https://huggingface.co/docs/transformers/index

---

## Addendum: Real-Time Neural Inferencing

---

## RTNeural: Fast Neural Inferencing for Real-Time Systems (Chowdhury, 2021)
**URL:** https://arxiv.org/abs/2106.03037
**PDF:** https://arxiv.org/pdf/2106.03037
**GitHub:** https://github.com/jatinchowdhury18/RTNeural
**License:** BSD 3-Clause (open source)

A C++ neural inferencing library designed specifically for hard real-time constraints — emphasis on speed, flexibility, small footprint, and ease of use. Covers the design motivation, real-world use cases, and performance benchmarks against other inferencing libraries (PyTorch C++, TensorFlow C++ API).

**Key architecture details:** Supports Dense (fully-connected), Conv1D, LSTM, GRU layers, and activation layers (tanh, ReLU, Sigmoid, SoftMax). Provides both a run-time API (dynamic model loading from JSON) and a compile-time API (fixed architecture with superior performance). Backends: Eigen (best for larger networks), xsimd (faster for smaller networks), STL (cross-platform fallback).

**Performance:** Outperforms PyTorch C++ API for small layer sizes. Switching from a custom inferencing engine to RTNeural yielded a ~3x performance improvement in ChowCentaur (Klon Centaur guitar pedal emulation). Network inference runs well above real-time audio threshold.

**Real-world use cases documented:** ChowCentaur (Klon Centaur distortion modelling), CHOWTapeModel (reel-to-reel analog tape physical modelling). Both are production audio plugins using recurrent neural networks at audio rate.

**SOS / SEISCLAW relevance:** Direct application for deploying trained seismic-to-audio mapping networks in low-latency real-time systems. Enables running pre-trained LSTM/GRU networks (trained offline on seismic waveform data) at audio sample rates with minimal CPU overhead — the C++ layer is the inference bridge between Python-trained models and live audio output.

---

*Compiled February 27, 2026*
