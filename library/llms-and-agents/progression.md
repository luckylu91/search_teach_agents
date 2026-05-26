---
domain: llms-and-agents
last_updated: 2026-05-26
target_level: intermediate
---

# LLMs and agents — Learning progression

Personal long-term tracking for the LLM + agent practices domain (job-relevant: GanAI
team at Antidot, FluidTopics chatbot + MCP + translation).

## Mastered

_(none recorded yet — populate as topics are confirmed)_

## In progress

- **Four-axis framing of the LLM field** — pre-training / post-training / utilization /
  evaluation, per [[overview]]. Frame is understood; specific techniques per axis are
  still to be filled in.
- **Current capabilities snapshot** — orientation-level grasp of what frontier LLMs and
  LLM-agents can do, per [[current-capabilities]]. Several items in that file are marked
  as "general field knowledge" and need dedicated sources before they count as covered.

## Open questions

- What specific RL variants and reward modeling approaches does Zhao 2026 single out in
  the post-training section? (Body of the survey is paywalled — needs the PDF.)
- Where does long-horizon agentic coding (Claude Code, Cursor agents, OpenHands) sit in
  the survey's "utilization" taxonomy, if it's covered at all?
- What is the survey's stance on evaluation of agents specifically (as opposed to
  base-LLM reasoning benchmarks)?

## Next steps

- Acquire the full PDF of [[zhao-2026-survey-of-llms]] via institutional access, run
  the `download-source-pdf` skill, then re-run synthesis with body content (not just
  the abstract).
- Add complementary sources for the unsourced claims in [[current-capabilities]] —
  candidates: a tool-use/MCP source, a RAG source (relevant to FluidTopics work),
  a long-context benchmark source, and a recent agent-evaluation source.
- Consider a separate domain or sub-area for **agentic coding** specifically, since
  that's where the GanAI team's work is increasingly heading.

## Session log

- 2026-05-26 — Domain created. Ingested Zhao et al. 2026 LLM survey via CrossRef
  metadata (full text paywalled). Wrote `overview.md` (four-axis frame) and
  `current-capabilities.md` (capability snapshot, conservative citations).
