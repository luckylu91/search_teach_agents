---
name: current-capabilities
domain: llms-and-agents
derived_from:
  - zhao-2026-survey-of-llms
status: draft
---

# Current LLM and agent capabilities — short description

A tight snapshot of what a competent practitioner can assume from frontier LLMs and
LLM-based agents as of mid-2026. The frame and the "utilization" categorization are
derived from [[overview]]; claims about specific abilities trace to the abstract of
the Zhao 2026 survey and are deliberately conservative — anything not in that source
is marked as **general field knowledge** so it can be challenged or replaced once more
sources are added.

## What LLMs themselves can do (capabilities baked in by training)

- **Core language capabilities** — fluent generation, comprehension, translation,
  summarization, and stylistic control across many languages and registers.
  *(survey, "evaluation methods" axis)*
- **Reasoning** — multi-step problem solving, including chains/trees of intermediate
  steps, treated as a first-class evaluation dimension.
  *(survey, "evaluation methods" axis)*
- **Safety / alignment behaviors** — refusals, calibrated uncertainty, instruction
  following, established primarily through post-training (SFT + RL family).
  *(survey, "post-training techniques" axis)*
- **In-context learning** — adaptation to new tasks from examples in the prompt,
  without weight updates.
  *(survey, "utilization strategies" axis)*

## What agents built on top of LLMs can do (the "utilization" stack)

The survey groups these under "utilization strategies that … enable effective
interaction with external environments":

- **Prompt engineering** — structured templating, few-shot exemplars, role conditioning.
  *(survey)*
- **Agentic reasoning** — planning, tool selection, and interaction with external
  environments. The abstract groups this with prompt engineering and in-context
  learning rather than treating it as a separate paradigm.
  *(survey)*

### General field knowledge (not yet sourced — needs sources added)

> These are widely accepted in practice but cannot be cited from the current library.
> Mark them as needing a dedicated source if they're used in teaching material.

- **Tool use / function calling** — LLMs reliably emit structured tool calls (JSON
  schemas, MCP servers) and consume their results.
- **Long-horizon agent loops** — multi-turn, multi-tool execution including code
  writing, code running, file system manipulation (Claude Code, Cursor agents, etc.).
- **Retrieval-augmented generation (RAG)** — hybrid keyword + semantic retrieval to
  ground answers in private corpora; relevant to Antidot's FluidTopics chatbot stack.
- **Multimodal input** — image, audio, and video understanding; some frontier models
  also generate images/audio.
- **Long context** — context windows in the 1M-token range are routine on frontier
  models; useful for whole-codebase reasoning.

## What they're still bad at (general field knowledge)

- Reliable factual recall without retrieval; LLMs hallucinate confidently.
- Multi-step arithmetic and exact symbolic manipulation without tool use.
- Tasks that require persistent state across sessions without an external memory layer.
- Truly novel reasoning that wasn't latent in pre-training — alignment can shift
  behavior, but not add knowledge.

## How to read this file

This is a working draft for orientation, not a benchmark report. Treat any claim
without a `(survey)` citation as folk knowledge until a dedicated source is added to
`library/llms-and-agents/sources/` (e.g. a benchmark paper, a method-specific survey,
or a vendor capability spec).

## See also

- [[overview]] — the four-axis framing this snapshot lives inside.
