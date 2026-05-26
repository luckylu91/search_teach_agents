---
name: current-capabilities
domain: llms-and-agents
derived_from:
  - zhao-2026-survey-of-llms
status: draft
---

# Current LLM and agent capabilities — short description

What a competent practitioner can assume from frontier LLMs and LLM-based agents
as of mid-2026. The four-axis frame comes from [[overview]]; this file collapses
that into "what works" / "what helps" / "what doesn't" terms. Citations point at
the survey's section numbers where the body content backs the claim.

## What LLMs reliably do (training-grounded capabilities)

- **Core language**: fluent generation, comprehension, translation, summarization,
  stylistic control across many languages and registers. (§6.1.1)
- **Reasoning**: multi-step problem solving, commonsense + mathematical + code,
  treated as a first-class evaluation dimension. Frontier models now exceed human
  performance on saturating benchmarks like MMLU (>90% for DeepSeek-R1) — meaning
  "good at standardized reasoning" is now table stakes, not a frontier claim. (§6.1.1, §6.3)
- **Long context**: 128K is now standard across LLaMA-3.1, Qwen2.5, DeepSeek-V3,
  Gemma 3, Qwen 3, GLM 4.5, Kimi K2 (per Table 2 of the survey). Achieved via
  RoPE-family PE extension + mid-training on 32K–200K sequences. (§3.2.2, §3.4)
- **In-context learning**: adaptation from a few exemplars in the prompt, no
  weight updates. Underlying mechanism: task recognition + task learning, with
  Transformer blocks inducing low-rank "learning-like" updates to internal MLP
  representations during forward inference. (§5.1.2)
- **Instruction following + safety behaviors**: refusals, calibrated uncertainty,
  helpfulness/harmlessness/honesty (3H) — established via SFT + RL post-training
  (RLHF, Constitutional AI / RLAIF, Safe RLHF). (§4.1, §4.2)
- **Long-CoT "slow thinking"**: extended in-context reasoning trace with iterative
  self-correction, popularized by OpenAI o1 and DeepSeek-R1. Trained primarily
  via RLVR on verifiable domains (math, code). Notably, the survey reports that
  RLVR **reshapes and narrows the action space** rather than expanding inherent
  capacity — so the base model still sets the ceiling. (§4.2.3, §5.2.1)
- **Tool use / agentic reasoning** is now a trained-in capability: the survey
  flags "agentic instruction synthesis" (§4.1.1) and "agentic training" (§4.3.2)
  as dedicated stages in pipelines like DeepSeek-V3.2 and Kimi-K2.5.

## What helps at utilization time (no weight changes)

- **Prompt engineering**: clarity, task description, input/output formats,
  few-shot demonstrations, role cues, multi-turn refinement, retrieval-grounded
  context. (§5.1.1)
- **CoT prompting**: zero-shot "Let's think step by step" still useful for non-
  reasoning-model LLMs; counterproductive for long-CoT models like o1 — the
  survey explicitly recommends avoiding CoT prompts in that case. (§5.1.3)
- **Test-time scaling**: Best-of-N / self-consistency / verifier-ranked sampling;
  tree-search and MCTS over reasoning states. The recurring trade-off is
  inference compute ↔ answer quality. (§5.2.1)
- **RAG**: standard retrieve → prompt-construct → generate; reranking and
  summarization to fight "lost in the middle"; iterative retrieval for multi-hop.
  Directly relevant to FluidTopics-style documentation chatbots. (§5.2.2)
- **Agentic systems**: memory + planning + execution loop, tool integration,
  long-context management strategies. See [[agentic-systems]] for the full breakdown.

## What still doesn't work reliably (per §7 + §6.3)

- **Factual recall** without retrieval — Transformer architectures show
  weakness in factual recall and compositional reasoning (§7, "Architecture
  innovations").
- **Long-horizon agent execution** — current agents show myopic planning and
  limited error recovery; memory + state management are still active problems
  (§7, "Agentic AI"; §5.3.3).
- **Uncertainty quantification + robustness** — hallucinations and inconsistent
  responses are surfaced as one of the eight major open challenges (§7,
  "Alignment and safety").
- **Practical-utility gap** — high benchmark scores ≠ real-world deployability;
  benchmark conditions don't predict performance under input variation
  (§6.3, "Practical utility").
- **Data-efficient training** — LLMs need trillions of tokens to acquire
  capabilities a human gets from far less; mechanistic understanding of how
  capabilities form is missing (§7, "Training efficacy").

## Strategic implications (job-relevant — GanAI / FluidTopics)

These are *my* inferences from the survey content, not survey claims:

- **RAG is in the survey's central frame** (§5.2.2) — the FluidTopics chatbot
  work fits squarely in the "utilization" layer the survey foregrounds.
- **Agentic chatbot replacing hard-coded RAG** = moving from §5.2.2 to §5.3 —
  the architectural jump is real but the survey treats both as utilization
  strategies on the same continuum.
- **MCP / tool use** isn't named in the survey but is implicitly the §5.3
  "execution component integrating external tools" — alignment with survey
  framing is clean.
- **Document translation** lives entirely in §6.1.1's "conditional generation"
  basic ability; the work there is product/UX more than model frontier.

## See also

- [[overview]] — the four-axis frame this snapshot lives inside.
- [[agentic-systems]] — focused note on the agent layer.
