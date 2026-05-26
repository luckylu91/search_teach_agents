---
name: overview
domain: llms-and-agents
derived_from:
  - zhao-2026-survey-of-llms
status: draft
---

# LLMs and agents — four-dimensional overview

A structuring frame for the domain, lifted directly from the Zhao et al. (2026) survey.
The survey organizes the LLM field into four orthogonal axes; treating them separately is
useful both for triaging new sources and for building a course.

## Summary

Modern LLM research splits cleanly into four concerns: how the base model is **trained**,
how it is **adapted** post-training, how it is **used** at inference time (including in
agentic settings), and how it is **evaluated**. Most production decisions involve trade-offs
within one of these axes; most research breakthroughs involve a new technique on one axis
that ripples through the others.

## Key ideas

### 1. Pre-training methodologies
- Core model capabilities are established here, via large-scale self-supervised training.
- Three sub-levers: training **methodology**, **architectural innovations**, and
  **data curation strategies**.
- This is where "what the model knows" is fixed; later stages can shift behavior but
  rarely add knowledge that wasn't latent.

### 2. Post-training techniques
- Adapt foundation models to downstream tasks and shape **alignment and safety**.
- Two main families called out: **supervised fine-tuning** and **reinforcement learning**
  (which in current practice covers RLHF, DPO, and successors).
- Where most product-facing personality, tool-use formatting, and refusal behavior is set.

### 3. Utilization strategies
- Optimize **real-world deployment** without changing weights.
- The survey explicitly groups three things here: **in-context learning**, **prompt
  engineering**, and **agentic reasoning** — i.e. the agent layer is treated as a
  utilization strategy, not a separate paradigm.
- This is the layer where Antidot's chatbot, MCP, and translation workflows live.

### 4. Evaluation methods
- Benchmarks across the key ability dimensions: **core language capabilities**, **reasoning**,
  and **safety**.
- Goal is "comprehensive and reliable assessment" — i.e. the survey treats evaluation as
  a first-class concern, not an afterthought.

## Open questions

- The published abstract is the only ingested text; the body of the survey almost certainly
  decomposes each dimension further (e.g. which specific RL variants, which agent
  architectures it considers canonical). Filling those in requires the full PDF.
- The survey was finalized for publication in early 2026; rapid-moving topics (long-horizon
  agentic coding, computer-use agents, MCP-style tool ecosystems) may be underrepresented
  even though the field treats them as central.
- The framing puts agents under "utilization," which is defensible but contested — others
  treat agentic systems as a separate research stack with its own training/eval loop.

## See also

- [[current-capabilities]] — a tighter, what-can-they-actually-do snapshot drawn from
  the same source plus general field knowledge.
