---
name: agentic-systems
domain: llms-and-agents
derived_from:
  - zhao-2026-survey-of-llms
status: draft
---

# Agentic systems — focused synthesis

Cross-cutting note on the agent layer, pulling content from §3.4 (mid-training),
§4.1.1 + §4.3.2 (agentic training), §5.3 (agentic systems proper), and §7
(agentic AI as open challenge). The survey treats agents not as a separate stack
but as a **utilization strategy** (§5) supported by dedicated training stages —
that framing is itself a useful anchor.

## Anatomy of an LLM agent (§5.3.1)

Three functional components, integrated into one feedback loop:

- **Memory**
  - *Short-term*: the LLM's context window — transient observations, intermediate
    reasoning.
  - *Long-term*: persists across interactions. Stores factual knowledge, past
    experiences, user profiles, agent configurations. Typically implemented via
    external storage + retrieval modules; updates done through reflection-based
    processes.
- **Planning**: decomposes complex tasks into sub-tasks, generates/evaluates/
  refines action plans iteratively based on environmental feedback. Essential
  for multi-step reasoning, delayed rewards, dynamic adaptation.
- **Execution**: carries out the planned actions. Basic execution can be purely
  textual/symbolic; advanced execution integrates external tools — search
  engines, APIs, databases, code interpreters — extending the model beyond
  parametric knowledge.

**Workflow loop**: perceive state → retrieve from memory → plan next action →
execute (possibly invoking tools) → receive environment feedback → store
selectively in memory → repeat. (§5.3.1)

## Advanced techniques (§5.3.2)

Three thrusts identified by the survey:

- **Long-context management** — two complementary paradigms:
  - *In-context compression and reconstruction*: hierarchical folding,
    iterative synthesis (IterResearch — condenses history into an evolving
    report and reconstructs the reasoning workspace).
  - *External memory*: targeted attention over persistent memory, reversible
    memory, sparse context invocation — selectively retrieve only what's
    relevant to the current decision.
- **Agentic self-evolution** — moving from static inference systems to
  progressively adaptive entities. Experience-driven learning is the dominant
  flavor: online interactions, historical trajectories, curated replay, and
  task-specific evaluation loops feed back into planning/decision/reasoning
  policy updates, often via external memory.
- **Multi-agent collaboration**:
  - *Centralized*: global orchestrator coordinates subordinate agents
    (task decomposition, role assignment, sequencing).
  - *Decentralized*: peer-to-peer negotiation and communication.
  - *Agent swarms* (Kimi-K2.5): dynamically spawn many sub-agents in parallel
    for sub-tasks — reduces runtime, increases throughput on multi-step
    reasoning and data synthesis.

## Application paradigms (§5.3.3)

Long-horizon agents have specialized into several recognizable shapes:

- **Deep search** — multi-step web navigation + information retrieval; combines
  structured reasoning, tool interaction, and RL to scale tool usage during
  inference. Emphasizes efficient exploration of large external information
  spaces under limited interaction budgets.
- **Deep research** — extends deep search toward comprehensive knowledge
  synthesis: iteratively collects evidence, refines hypotheses, produces
  citation-grounded reports. Emphasizes long-range consistency, evidence
  integration, structured report generation. *(This is conceptually what the
  teacher_agent platform itself is doing — note the alignment.)*
- **Code agents** — long-horizon SWE / code-analysis tasks. Reason over large
  codebases, iteratively execute and debug, integrate environment feedback.
  Requires persistent state tracking, error recovery, incremental refinement.
  Evaluated via SWE-bench (§6.1.2).

The survey also flags **wide reasoning** as an emerging direction: explore many
candidate paths / subtasks in parallel (vs. deep linear exploration), integrating
depth and width.

## Training pipeline for agents (cross-§)

Agentic capability isn't only a utilization-time thing — the survey treats it
as a first-class training target:

- **Mid-training (§3.4)** seeds agentic capability with curriculum that
  progressively introduces tool-use, long-context, and agent-specialized data
  (Step-3.5-Flash trains nearly 1T tokens through a staged sequence from general
  text → code → tool-use → long-context → agent-specialized).
- **Agentic instruction synthesis (§4.1.1)** is its own SFT data type: build
  dedicated environments + tool repositories, have a teacher model generate
  multi-turn trajectories, filter with LLM-as-judge, supplement with real
  execution sandboxes. The instruction-data format must specify how
  intermediate reasoning, tool calls, and environment observations are
  interleaved.
- **Agentic RL (§4.3.2)** is now a dedicated post-training stage. For each
  task, the model acts as a policy generating interleaved
  reasoning/tool-call/observation trajectories; optimization uses trajectory-
  level rewards from rule-based functions (success checks, unit tests,
  rubrics) or LLM-as-judge evaluators. **Significantly more resource-intensive
  than standard RL** — needs specialized infrastructure and asynchronous
  training systems (decoupled trajectory generation from policy updates,
  partial-rollout training).

Examples from the survey: DeepSeek-V3.2 uses a cold-start phase unifying
reasoning + tool use within single trajectories, plus large-scale agentic task
synthesis. Kimi-K2.5 builds a unified multimodal agentic RL framework with
outcome- and trajectory-based evaluators, plus the *agent swarm* approach.

## Open challenges (§7, "Agentic AI")

Explicitly called out by the survey as unsolved:

- **Long-horizon execution** — myopic planning, limited recovery from errors
  or unexpected tool outputs.
- **Context and memory management** — agentic tasks involve extensive
  environmental interaction and require persistent, well-structured context.
- **Architecture** — needs dedicated components for memory, state management,
  execution verification.
- **Training infrastructure** — current agentic-training environments are
  hard to construct and resource-intensive to scale.

The survey's framing: "the path forward requires a holistic co-evolution of
foundation models, reasoning algorithms, and system infrastructure."

## Practical takeaways (mine, not the survey's)

- The "memory + planning + execution" decomposition (§5.3.1) maps cleanly onto
  how the agents in this very repo (`.claude/agents/`) are structured —
  explorer/synthesizer/course-author do planning + execution; the `library/`
  is the long-term memory layer.
- For the GanAI team's transition from hard-coded RAG to an agentic chatbot:
  the survey's "deep search" application paradigm (§5.3.3) is the closest
  archetype, and the tool-use formatting + agentic RL training implications
  matter even if you don't train your own.
- MCP servers exposing semantic/keyword search fit the survey's "execution
  component integrating external tools" pattern verbatim.

## See also

- [[overview]] — the four-axis frame.
- [[current-capabilities]] — operational snapshot.
