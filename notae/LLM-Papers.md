# The Top 26 Essential LLM Papers + 5 Bonus Resources

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

## 4. Language Models are Few-Shot Learners (GPT-3) (Brown et al., 2020)
**URL:** https://arxiv.org/abs/2005.14165

Demonstrated in-context learning at scale with 175B parameters. Shifted understanding of how prompting works and proved LLMs could perform tasks without fine-tuning through few-shot examples.

---

## 5. Scaling Laws for Neural Language Models (Kaplan et al., 2020)
**URL:** https://arxiv.org/abs/2001.08361

First empirical framework relating model performance to parameters, dataset size, and compute. Established power-law relationships but underestimated optimal token counts (corrected by Chinchilla).

---

## 6. Training Compute-Optimal Large Language Models (Chinchilla) (Hoffmann et al., 2022)
**URL:** https://arxiv.org/abs/2203.15556

Proved that for fixed compute budgets, token count matters more than parameter count. Showed most large models were undertrained, fundamentally changing scaling strategies.

---

## 7. LLaMA: Open and Efficient Foundation Language Models (Touvron et al., 2023)
**URL:** https://arxiv.org/abs/2302.13971

Triggered the open-weight model era. Established modern architectural defaults: RMSNorm, SwiGLU activation, and RoPE positional encoding as standard practice.

---

## 8. RoFormer: Enhanced Transformer with Rotary Position Embedding (Su et al., 2021)
**URL:** https://arxiv.org/abs/2104.09864

Introduced Rotary Position Embedding (RoPE), now the default positional encoding for long-context LLMs. Enables better extrapolation to longer sequences than seen during training.

---

## 9. FlashAttention: Fast and Memory-Efficient Exact Attention (Dao et al., 2022)
**URL:** https://arxiv.org/abs/2205.14135

IO-aware attention algorithm that optimizes GPU memory access patterns. Enabled practical long context windows and high-throughput inference by reducing memory bottlenecks.

---

## 10. Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks (Lewis et al., 2020)
**URL:** https://arxiv.org/abs/2005.11401

Combined parametric language models with non-parametric external knowledge retrieval. Foundational architecture for grounded generation and enterprise AI systems.

---

## 11. Training Language Models to Follow Instructions with Human Feedback (InstructGPT) (Ouyang et al., 2022)
**URL:** https://arxiv.org/abs/2203.02155

The RLHF blueprint that created instruction-following models. Established supervised fine-tuning + reward modeling + PPO as the standard post-training alignment pipeline.

---

## 12. Direct Preference Optimization: Your Language Model is Secretly a Reward Model (Rafailov et al., 2023)
**URL:** https://arxiv.org/abs/2305.18290

Simplified RLHF by eliminating the separate reward model and unstable RL training. Directly optimizes preferences through a stable classification loss function.

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

Reignited Mixture of Experts for modern deep learning. Demonstrated conditional computation at scale, activating only subset of parameters per token.

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

Demonstrated high-quality synthetic data allows small models to outperform much larger ones. Quality over quantity in training data curation.

---

## 23. Scaling Monosemanticity: Extracting Interpretable Features from Claude 3 Sonnet (Templeton et al., 2024)
**URL:** https://transformer-circuits.pub/2024/scaling-monosemanticity/

Major breakthrough in mechanistic interpretability using sparse autoencoders. Decomposes neural activations into millions of interpretable, human-understandable features.

---

## 24. PaLM: Scaling Language Modeling with Pathways (Chowdhery et al., 2022)
**URL:** https://arxiv.org/abs/2204.02311

Masterclass in distributed training orchestration across 6,144 TPU chips. Demonstrated efficient scaling to 540B parameters with careful engineering.

---

## 25. GLaM: Efficient Scaling of Language Models with Mixture-of-Experts (Du et al., 2022)
**URL:** https://arxiv.org/abs/2112.06905

Validated MoE economics: 1.2T total parameters with only 97B active per token. Matched dense model quality at fraction of inference cost.

---

## 26. The Smol Training Playbook (Hugging Face, 2025)
**URL:** https://github.com/huggingface/smol-vision/blob/main/README.md

Practical end-to-end guide for efficiently training small language models. Covers data curation, architecture choices, training strategies, and evaluation.

---

# Bonus Material

## T5: Exploring the Limits of Transfer Learning (Raffel et al., 2019)
**URL:** https://arxiv.org/abs/1910.10683

Unified NLP tasks into text-to-text format. Comprehensive study of transfer learning with pre-training objectives, architecture variants, and dataset scaling.

---

## Toolformer: Language Models Can Teach Themselves to Use Tools (Schick et al., 2023)
**URL:** https://arxiv.org/abs/2302.04761

Models learn to use external tools (calculators, search, APIs) through self-supervised learning, deciding when and how to call tools.

---

## GShard: Scaling Giant Models with Conditional Computation (Lepikhin et al., 2020)
**URL:** https://arxiv.org/abs/2006.16668

Scaled Transformers to 600B parameters using MoE. Demonstrated efficient multilingual translation with conditional computation.

---

## Adaptive Mixtures of Local Experts (Jacobs et al., 1991)
**URL:** https://ieeexplore.ieee.org/document/6797059

Original MoE paper from early neural networks. Introduced gating networks that dynamically route inputs to specialized sub-networks.

---

## Hierarchical Mixtures of Experts and the EM Algorithm (Jordan & Jacobs, 1994)
**URL:** https://www.cs.toronto.edu/~hinton/absps/jjnh91.pdf

Extended MoE to hierarchical structures. Theoretical foundation for modern expert routing and modular neural architectures.
