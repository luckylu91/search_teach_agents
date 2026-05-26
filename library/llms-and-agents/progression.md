---
domain: llms-and-agents
last_updated: 2026-05-26
target_level: intermediate
---

# LLMs and agents — Learning progression

Personal long-term tracking for the LLM + agent practices domain (job-relevant:
GanAI team at Antidot, FluidTopics chatbot + MCP + translation).

## Mastered

- **Four-axis framing of the LLM field** — pre-training / post-training /
  utilization / evaluation, per [[overview]]. Frame is internalized and useful
  as a triage tool for new sources.
- **Anatomy of an LLM agent** — memory + planning + execution loop, per
  [[agentic-systems]]. Maps cleanly onto how `.claude/agents/` are structured
  in this very repo.

## In progress

- **Concrete techniques per axis** — overview.md now lists specific named
  techniques (GQA/MLA, MoBA/NSA, RoPE/YaRN, RLVR/GRPO/DPO, RAG variants,
  agent swarms, etc.). Familiarity is "I can recognize the name and place it
  on the axis"; deep grasp requires reading the cited subsections of the
  survey body and ideally one focused source per technique.
- **Industrial post-training pipelines** — DeepSeek-R1's four-stage recipe
  (long-CoT cold-start → long-CoT RL → hybrid SFT → hybrid RL) is the
  canonical pattern, per §4.3. Worth a deeper pass when adding more sources.
- **Agentic RL specifically** — the survey covers it but light on details;
  needs a dedicated source. Highly relevant to the team's agentic-chatbot
  pivot.

## Open questions

- How does the survey's "deep search" / "deep research" / "code agents"
  taxonomy (§5.3.3) line up with what's shipped in Claude Code, Cursor,
  OpenHands? The survey doesn't name those products by name.
- Long-context management: is in-context compression (IterResearch-style) or
  external memory the better default for FluidTopics-sized corpora?
- MCP isn't in the survey at all. Where does a tool-use protocol sit in the
  agent literature — is there a parallel academic concept, or is MCP a
  practitioner-only construct?
- Verifiable rewards for non-math/non-code tasks: rubric-based rewards (§4.2.2)
  are the survey's hint, but the actual recipes need a separate source.

## Next steps

- Add a dedicated source on **MCP** and tool-use protocols (Anthropic spec +
  any academic paper that frames similar problems).
- Add a dedicated source on **agentic RL** with infrastructure details — the
  survey points at DeepSeek-V3.2 and Kimi-K2.5 reports; pulling one of those
  in would substantially deepen [[agentic-systems]].
- Add a focused source on **RAG evaluation** — directly job-relevant for
  FluidTopics chatbot quality work, and the survey treats RAG only briefly
  in §5.2.2.
- Consider a separate domain for **agentic coding** (Claude Code, Cursor,
  OpenHands), since that's where the GanAI team's work is increasingly
  heading and the survey covers it only as one of three application
  paradigms.

## Session log

- 2026-05-26 — Domain created. Ingested Zhao et al. 2026 LLM survey via
  CrossRef metadata (abstract only). Wrote [[overview]] and
  [[current-capabilities]] from the abstract.
- 2026-05-26 — Full PDF added (Git LFS) and external markdown dump of the
  body placed under `sources/zhao-2026-survey-of-llms/`. Rewrote [[overview]]
  and [[current-capabilities]] with body-grounded content and section-level
  citations; added new focused note [[agentic-systems]] cross-cutting §3.4,
  §4.1.1, §4.3.2, §5.3, §7.
