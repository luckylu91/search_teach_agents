---
name: overview
domain: llms-and-agents
derived_from:
  - zhao-2026-survey-of-llms
status: draft
---

# LLMs and agents — four-dimensional overview

A navigational frame for the domain, lifted from Zhao et al. 2026. The survey
organizes the LLM field into four orthogonal axes; treating them separately is
useful for triaging sources, building courses, and reasoning about where a given
technique sits in the pipeline. Where useful below, I cite the survey's subsection
numbers so a later reader can jump straight to the relevant body content.

## Summary

Modern LLM work decomposes into four concerns, each with its own techniques and
trade-offs: how the base model is **pre-trained** (§3), how it is **adapted** post-
training (§4), how it is **used** at inference time including in agentic loops (§5),
and how it is **evaluated** (§6). The survey treats *mid-training* as an emerging
sub-stage between pre- and post-training where capabilities like long-context and
agentic reasoning are seeded (§3.4). The survey's own framing (§1) places the
"utilization" layer — prompt engineering + advanced inference + agents + efficient
deployment — as the operational layer where production systems actually live.

## 1. Pre-training (§3)

Establishes what the model knows. Three sub-levers, plus a fourth optional stage:

- **Data preparation (§3.1)** — general / specialized / synthetic corpora; a
  preprocessing pipeline of de-duplication, filtering, PII reduction, tokenization
  (BPE, vocab typically 100K–256K); a "data mix" + "data curriculum" decision
  problem. Frontier open models train on **15–30T tokens**; LLaMA-3.1 allocates
  ~25% to math/reasoning and 17% to code.
- **Architecture (§3.2)** — causal decoder-only is the de facto standard. Active
  research areas: sparse attention (MoBA, NSA, DSA), attention grouping/compression
  (GQA, MQA, MLA), linear attention (Mamba, RWKV, DeltaNet — typically mixed with
  full attention at ~3:1), RoPE-family positional encodings with length-extension
  variants (ABF, YaRN, LongRoPE), MoE with gating / shared-expert / load-balancing
  refinements, plus emerging designs (Hyper-Connections, memory augmentation,
  looped Transformer, diffusion LMs).
- **Training techniques (§3.3)** — parallelism (DP/TP/PP/MP/CP/EP), ZeRO, communication
  optimization (DualPipe, IBGDA/DeepEP), kernel optimization (FlashAttention,
  DeepGEMM), and mixed-precision training (FP16 → BF16 → FP8, with MXFP4 on Hopper/
  Blackwell). Optimizers: Adam/AdamW still standard; spectral-norm optimizers like
  Muon/SOAP gaining traction.
- **Mid-training (§3.4)** — a now-standard intermediate stage that injects
  reasoning-, coding-, long-context-, and agentic-task data with a staged curriculum;
  also where the context window gets extended (commonly 32K → 200K+).

## 2. Post-training (§4)

Adapts the base model. Two technique families, often combined in industrial pipelines:

- **Supervised fine-tuning (§4.1)** — instruction tuning. Five typical instruction
  data types per the survey: math (20–30%), code (20–30%), agentic (10–20%),
  general (20–40%), and AI identity/safety (minor). Around **1M instances**
  typical for open-source SFT; "instruct" and "thinking" modes commonly merged via
  control flags (Qwen3's `/think` `/no_think`).
- **Reinforcement learning (§4.2)** — split into reward modeling (rule-based vs.
  model-based; RLHF, RLVR, rubric-based, process reward) and policy optimization
  (PPO → GRPO → variants; offline alternatives: DPO, RFT, OPD). DeepSeek-R1's
  four-stage pipeline (long-CoT cold-start → long-CoT RL → hybrid SFT → hybrid RL)
  is the canonical recipe for long-CoT reasoning models.
- **Industrial pipelines (§4.3)** — multi-stage, iterative, with **agentic
  training** now a dedicated stage in DeepSeek-V3.2, Kimi-K2.5, etc. Reward design
  bifurcates: preference-based (DPO-style) for open-ended helpfulness, verifiable
  rewards (RLVR) for math/code/agentic.

## 3. Utilization (§5)

What you do with a trained model. The survey's framing puts agents *inside* this
layer, not as a separate paradigm:

- **Basic prompting (§5.1)** — ICL, CoT (with "Let's think step by step"-style
  zero-shot now dominant; long-CoT models actually recommend *not* CoT-prompting),
  automatic prompt optimization.
- **Advanced inference-time techniques (§5.2)** — test-time scaling via
  Best-of-N / self-consistency, search (ToT, GoT, MCTS), and **long-CoT reasoning**
  (slow thinking, self-correction). Also RAG (§5.2.2): retrieve → prompt-construct
  with reranking/summarization to fight "lost in the middle" → generate, with
  iterative refinement variants.
- **Agentic systems (§5.3)** — memory + planning + execution loop. Advanced
  techniques: long-context management (in-context compression vs. external memory),
  self-evolution via experience-driven learning, and multi-agent collaboration
  (centralized orchestration vs. decentralized; "agent swarms"). Application
  paradigms: deep search, deep research, code agents. See [[agentic-systems]] for
  a focused note.
- **Efficient deployment (§5.4)** — the *memory wall* in autoregressive decoding;
  FlashAttention/PagedAttention/speculative decoding/early exit; KV cache
  optimization at token/model/system levels; compression (PTQ at 4-bit, distillation,
  pruning, lossless DFloat11).

## 4. Evaluation (§6)

The survey treats evaluation as a first-class concern with three axes:

- **Basic ability (§6.1.1)** — language, reasoning (HellaSwag, ARC, MATH, GSM8K,
  HumanEval, MBPP), alignment (IFEval, TruthfulQA), comprehensive (MMLU, BIG-bench,
  Chatbot Arena).
- **Advanced ability (§6.1.2)** — expert-level reasoning (AIME, OlymMATH, GPQA),
  agentic tasks (τ-Bench, SWE-bench, OSWorld) — the latter is a rapidly growing
  evaluation frontier.
- **Approaches (§6.2)** — benchmark-based, human-based (Chatbot Arena pairwise),
  model-based (LLM-as-judge, agent-as-a-judge).
- **Issues (§6.3)** — fairness/contamination, the benchmark-vs-practical-utility
  gap, and **saturation** (MMLU now >90% for frontier models — "further gains are
  becoming increasingly meaningless").

## Open challenges (§7)

The survey's own list of unresolved issues, useful both as research-direction map
and as a sanity check on what *not* to assume is solved:

1. **Theoretical foundations** — no first-principles theory of why next-token
   prediction produces general capability.
2. **Architecture innovations** — quadratic attention, factual recall, compositional
   reasoning still weak; needs algorithm × hardware co-design.
3. **Scaling enhancement** — must move from blunt-force expansion to directed,
   capability-aware scaling.
4. **Data barrier** — high-quality public text is largely exhausted; the path
   forward is data efficiency + verifiable programmatic data-generation environments.
5. **Training efficacy** — pre-training is data-inefficient relative to humans; we
   lack mechanistic understanding of how specific capabilities form.
6. **Alignment and safety** — hallucinations, uncertainty quantification, robustness;
   RLHF is one tool, not a solved problem.
7. **Capacity assessment** — contamination + benchmark-vs-utility gap + saturation.
8. **Agentic AI** — long-horizon planning, robust error recovery, context/memory
   management, infrastructure for scaling agentic RL all remain hard.
9. **Sustainability & responsibility** — energy, governance, equitable access.

## See also

- [[current-capabilities]] — operational "what can it do today" snapshot.
- [[agentic-systems]] — focused note on the agent layer, cross-cutting §3.4, §4.1,
  §4.3, §5.3, §7.
