# Case Study — rag-kb: A Trustworthy Retrieval Layer for a Solo AI-First Agency

**The problem.** Running an AI-first software agency alone generates a lot of state to track: what did we decide, what phase is each project in, what has each session cost. Generic tools don't fit this shape — code-search MCP servers (Cody, Cursor, Claude Context) answer "how does this function work," not "what did we decide about X." Enterprise RAG platforms (Onyx, PrivateGPT) solve the retrieval problem but at team/server weight, with no notion of SDD phase-state or AI cost. I surveyed both categories, confirmed the gap, and built for it.

**What I built.** `rag-kb` is a local-first, read-only MCP server that answers natural-language questions about a software project from its own Git-tracked documents — with citations, and with honest refusal: it returns `not_found` instead of guessing, and `stale` instead of answering from an outdated index. It runs inside Claude Code, so there's no server to deploy and no data leaving the machine.

**Results, measured, not claimed:**
- 88% correct, 0 fabrications on a held-out evaluation set (G1)
- 0.01s median retrieval latency
- 100+ tests, with `import-linter` architecture contracts and `ruff`/`mypy` enforced in CI

**How I built it.** Spec-Driven Development, phase-gated: Discovery → Requirements → Design → Build → Evaluation, each phase gated by a written spec and a human decision before the next one opens — not "prompt until it looks right." The refusal behavior (`not_found`/`stale`) was a design decision made in Discovery, before a line of retrieval code existed, because a RAG tool that occasionally invents an answer is worse than one that says "I don't know." The evaluation set existed before the eval numbers did.

**Why this matters.** Most AI-engineering portfolios show a demo. This shows a shipped system with a measured quality bar, a documented reason for every architectural choice, and a process disciplined enough to catch its own failure modes before a user does — the difference between "I can call an LLM API" and "I can ship an AI system someone can trust."
