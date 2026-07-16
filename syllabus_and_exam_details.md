# Claude Certified Architect – Foundations (CCA-F / CCAR-F)

Study guide and syllabus, compiled from Anthropic's official exam guide and cross-referenced against several independent prep resources. Anthropic's own guide and PDF are the authoritative source — treat everything below as a study aid, not a replacement for it.

> **Note on sources:** Search results for this certification are a mix of official Anthropic pages (`anthropic.skilljar.com`) and unofficial third-party prep sites, some of which sell practice questions or courses. This README draws only on details that were consistent across multiple independent sources. Verify exact figures against Anthropic's official exam guide PDF before relying on them for exam-day decisions.

## What it validates

The certification confirms that a practitioner can make informed tradeoff decisions when implementing real-world, production-grade solutions with Claude — not just recall facts about the API. It's scenario-based: every question is anchored to a realistic production situation rather than asked in isolation.

## Exam logistics

| Detail | Value |
|---|---|
| Format | 60 multiple-choice questions (4 options, 1 correct) |
| Duration | 120 minutes |
| Delivery | Online proctored or Pearson VUE test center |
| Scoring | Scaled score, 100–1,000 |
| Passing score | 720 |
| Fee | ~$125 USD |
| Validity | 12 months from award date |
| Badge | Digital Credly badge (LinkedIn-verifiable) |
| Access | Tied to the Claude Partner Network |
| Mode | Closed-book, no AI assistance |

## Exam scenarios

Each exam question is anchored to one of 6 possible production scenarios. **4 of the 6 are randomly selected** for your session, and each scenario tests 2–3 domains at once:

1. Customer Support Resolution Agent — high-ambiguity requests (returns, billing disputes, account issues), escalation logic
2. Multi-Agent Research System — coordinator delegates to specialized subagents (search, analysis, synthesis, reporting)
3. Developer Productivity Tools — codebase exploration, legacy system understanding, boilerplate generation, built-in tools + MCP
4. Code Generation with Claude Code — automated review, test generation, PR feedback
5. Claude Code for CI/CD — non-interactive mode, structured output in pipelines
6. Structured Data Extraction — unstructured documents → validated JSON, edge-case handling, downstream integration

## Domain weightings

| Domain | Weight |
|---|---|
| 1. Agentic Architecture & Orchestration | 27% |
| 2. Tool Design & MCP Integration | 18% |
| 3. Claude Code Configuration & Workflows | 20% |
| 4. Prompt Engineering & Structured Output | 20% |
| 5. Context Management & Reliability | 15% |

Domains 1 and 3 together make up nearly half the exam — prioritize them if your study time is limited, but don't neglect Domain 4, which is tied for second-highest weight.

---

## Syllabus by domain

### Domain 1 — Agentic Architecture & Orchestration (27%)

- Agentic loops for autonomous task execution — the perceive/act/observe cycle
- Agents vs. workflows vs. conversational systems — when each pattern is appropriate
- Multi-agent architecture: coordinator/subagent delegation patterns
- How subagents operate with **isolated context** — they do not inherit the coordinator's conversation history
- Iterative refinement loops: coordinator evaluates synthesis output for gaps, re-delegates with targeted queries
- Error propagation from subagent to coordinator — structured failure context vs. silent/generic failure handling
- Escalation triggers: explicit user request for a human, policy exceptions/gaps, inability to make progress
- Prompt chaining (fixed sequential steps) vs. dynamic decomposition (open-ended investigation) — matching the pattern to the task
- Session management: `resume`, `fork_session`, when a fresh session beats resuming a stale one

### Domain 2 — Tool Design & MCP Integration (18%)

- MCP architecture: host, client, server relationship (1 client per server)
- Core primitives: Tools (callable actions), Resources (contextual data), Prompts (reusable templates)
- Client-side capabilities: Sampling, Roots, Elicitation
- Writing tool descriptions — the primary mechanism the model uses for tool selection; minimal descriptions cause unreliable routing among similar tools
- What a good tool description includes: input formats, example queries, edge cases, explicit boundaries
- Error handling in agent tools: transient vs. permanent errors, structured error responses, uncertain-state handling
- MCP-specific error patterns and how they differ from generic API errors
- Transport layer: stdio (local) vs. Streamable HTTP (remote)
- Security and trust: user consent, scoped/least-privilege permissions, OAuth 2.1 for remote servers

### Domain 3 — Claude Code Configuration & Workflows (20%)

- Claude Code architecture: tool system, execution model, built-in tools (Read, Write, Bash, Grep, Glob)
- Project setup, context management, and session continuity
- `CLAUDE.md` hierarchy: user, project, and team-level precedence; path-specific rules; subdirectory config; `.claudeignore`
- Writing effective `CLAUDE.md` instructions for teams
- Custom slash commands: creation, structure, distribution (`.claude/commands/`)
- Skills: `SKILL.md`, frontmatter, triggers, enterprise deployment
- Subagents in Claude Code: configuration, scope, delegation
- Hooks: lifecycle events, implementation, exit code conventions; common patterns (logging, safety nets, automation)
- Plan mode vs. direct execution — plan mode for large-scale, multi-file, architecturally ambiguous changes; direct execution for well-scoped real-time tasks
- Claude Agent SDK: programmatic session control
- CI/CD integration: the `-p`/`--print` flag for non-interactive mode, `--output-format json` and `--json-schema` flags, why the same session that generated code is a weaker reviewer of its own changes (no independent context)
- Multi-instance and multi-pass review architecture: independent review instances catch more than self-review; splitting large reviews into per-file + cross-file integration passes to avoid attention dilution

### Domain 4 — Prompt Engineering & Structured Output (20%)

- Core prompt engineering: clarity, positive/negative examples, step-by-step reasoning
- Advanced techniques: the "interview pattern" to surface design considerations (e.g. cache invalidation, failure modes) before implementation in unfamiliar domains
- Providing concrete test cases (example input/expected output) to fix edge-case handling
- Bundling multiple interacting fixes into a single detailed message vs. sequential iteration for independent issues
- Structured output: JSON schema validation, `nullable: true` vs. `required` for uncertain/missing fields
- Designing prompts that minimize false positives in automated review/feedback systems
- Evaluation design: how to validate structured output against schemas and catch edge cases before they reach downstream systems

### Domain 5 — Context Management & Reliability (15%)

- Managing context windows across long documents, multi-turn conversations, and multi-agent handoffs
- The "lost in the middle" effect — models reliably process information at the beginning and end of long contexts, less reliably in the middle
- Extracting transactional facts (amounts, dates, IDs, statuses) into a persistent "case facts" block, kept outside summarized history
- Progressive summarization strategies for long-running sessions
- Synchronous API vs. Batch API: matching approach to workflow latency (synchronous for blocking pre-merge checks, batch for overnight/weekly analysis)
- Batch API limitations: no multi-turn tool calling within a single batch request
- `custom_id` fields for correlating batch request/response pairs and resubmitting only failed items
- Calculating batch submission frequency against SLA constraints
- Prompt refinement on a small sample before scaling to a full batch run, to maximize first-pass success rate
- Reliability patterns: confidence scoring, graceful degradation, escalation under uncertainty

---

## Study plan (suggested — adjust to your baseline)

| Timeframe | Focus |
|---|---|
| Week 1 | Read the official exam guide PDF end to end. Skim all 5 domains and 6 scenarios once before going deep on any one. |
| Week 2 | Domain 1 (Agentic Architecture) + Domain 3 (Claude Code) — these two domains are 47% of the exam combined. Build or trace through a small multi-agent example if you haven't already. |
| Week 3 | Domain 4 (Prompt Engineering & Structured Output) + Domain 2 (MCP). Practice writing tool descriptions and JSON schemas by hand, not just reading about them. |
| Week 4 | Domain 5 (Context & Reliability). Take a full practice exam under timed conditions. Review every wrong answer's rationale — the "why," not just the correct letter. |
| Final days | Re-read task statements for your weakest domain. Do a light pass over all 6 scenarios so none of them are unfamiliar on exam day. |

**General guidance from practitioners who've taken it:**
- The exam rewards recognizing tradeoffs, not memorizing definitions — expect "which approach is best given constraint X" questions, not "what does parameter Y do."
- A recurring pattern: when a question asks how to enforce ordering, compliance, or policy rules, the architecturally correct answer is usually the deterministic/programmatic one (hooks, schema validation, explicit checks) over the probabilistic one (prompt instructions, few-shot examples alone).
- Read the rationale for *every* practice question you get wrong — several sources independently call this the single highest-value study activity.

## Resources

- Official exam guide PDF and access request — via Anthropic Partner Academy (`anthropic.skilljar.com`)
- Anthropic's own documentation: `platform.claude.com/docs` (API, Agent SDK) and `code.claude.com/docs` (Claude Code) — note these replaced the older `docs.anthropic.com` paths
- Anthropic's official practice exam (available through the certification portal) — includes explanations after each answer

---

*This document was compiled from publicly available descriptions of the exam guide and independent study resources as of July 2026. Domain weightings, scenario names, and format details should be cross-checked against Anthropic's current official exam guide before you rely on them, since certification programs are updated over time.*
