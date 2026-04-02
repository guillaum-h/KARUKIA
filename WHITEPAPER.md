# KARUKIA — Whitepaper

**AI-assisted development methodology: 11 audit dimensions via the MCP protocol.**

*Version 3.1 — Document in English. Version française : [LIVRE-BLANC.md](./LIVRE-BLANC.md)*

```
██╗  ██╗ █████╗ ██████╗ ██╗   ██╗██╗  ██╗    ██╗ █████╗
██║ ██╔╝██╔══██╗██╔══██╗██║   ██║██║ ██╔╝    ██║██╔══██╗
█████╔╝ ███████║██████╔╝██║   ██║█████╔╝     ██║███████║
██╔═██╗ ██╔══██║██╔══██╗██║   ██║██╔═██╗     ██║██╔══██║
██║  ██╗██║  ██║██║  ██║╚██████╔╝██║  ██╗    ██║██║  ██║
╚═╝  ╚═╝╚═╝  ╚═╝╚═╝  ╚═╝ ╚═════╝ ╚═╝  ╚═╝    ╚═╝╚═╝  ╚═╝
   AI methodology for highly regulated industries · Made in Guadeloupe 🇬🇵
```

---

## 1. The Problem

AI assistants (Claude, GPT, Copilot) are powerful but generic. When asked for a security audit, the AI improvises: it knows OWASP concepts but doesn't follow a structured methodology. Results vary from session to session with no guaranteed exhaustiveness.

**Real-world consequences:**

- Vulnerabilities slip through because the AI "forgets" to check certain points
- No traceability: impossible to prove a control was performed
- No reproducibility: two audits of the same code yield different results
- Zero compliance: frameworks (ISO 27001, SOC 2, HDS) require formal evidence
- No coverage beyond security: TypeScript quality, architecture, performance, and technical debt are invisible

## 2. The KARUKIA Solution

KARUKIA solves this by injecting a **complete methodology** directly into the AI's context, on demand.

### How it works

KARUKIA is an MCP server (Model Context Protocol) — Anthropic's open standard for connecting tools to AI. When the user requests an audit, the MCP server returns a **monolithic prompt** containing:

1. **Persona identity** — name, expertise, communication style
2. **Workflow** — mandatory steps, in order, with validation gates
3. **Checklists** — all relevant control points, inline in the prompt
4. **Templates** — expected output format (tables, scores, verdicts)
5. **Guard rails** — non-negotiable rules (e.g., "every finding must cite file:line")

The AI receives this prompt and **becomes** the specialist for the duration of the session. It cannot "forget" a control — it's written in its context.

### The MCP Protocol

The [Model Context Protocol](https://modelcontextprotocol.io/) is an open standard that allows any AI client to connect to tool servers. KARUKIA is built for and tested with **Claude Code** and **OpenAI Codex**. It is compatible with any MCP client (Cursor, Windsurf, Copilot, etc.).

Installation is a single line:

```bash
npx karukia-mcp
```

No account, no API key, no data sent externally. The server runs locally on the developer's machine.

---

## 3. Architecture

### Local transport (default)

```
Developer <-> AI Client <-> [stdio] <-> KARUKIA MCP Server (local)
                  |                         |
                  |                         ├── 20 skills (prompt builders)
                  |                         ├── Shared blocks:
                  |                         │     GUARD + WORKFLOW + COVERAGE + AGENTS
                  |                         ├── Platform abstraction (client_id):
                  |                         │     Claude / Codex / Generic profiles
                  |                         ├── 22 checklists (1800+ points)
                  |                         └── Memory system:
                  |                               sessions/ + coverage/ + knowledge/ + trackers/
                  |
             Claude Code
             OpenAI Codex
             Any MCP client
```

The server communicates via **stdio** (standard input/output). No network port opened, no HTTP calls. Everything stays on the developer's local machine.

### What the server returns

Each MCP tool returns a **monolithic prompt** — a single text block containing everything the AI needs. No chain of calls, no RAG, no vector database. Just a well-constructed prompt.

Simplified example for `neo` (security audit):

```
[GUARD — non-negotiable obligations]
+
[Neo persona — identity, style, expertise]
+
[WORKFLOW — standardized 6-step process]
+
[COVERAGE — load previous scan manifest, prioritize unscanned files]
+
[OWASP checklist — 62 inline controls]
+
[ISO 27001 checklist — 93 inline controls]
+
[AGENTS — multi-agent parallel exploration strategy]
+
[Output template — table format + verdict]
```

The WORKFLOW and COVERAGE blocks are new in v3.1 — they turn every audit from a one-shot scan into an **iterative, cumulative process** where no file in the codebase is left behind.

---

## 4. The 12 Audit Dimensions

### Security — Defensive (Neo)

Point-by-point audit against 6 compliance frameworks. Every finding cites file:line. Every verdict is COMPLIANT or NON-COMPLIANT with evidence.

| Framework | Controls | Scope |
|-----------|----------|-------|
| OWASP Security Baseline | 62 | Every web app |
| HDS 2.0 | 52 | Health data (France) |
| ISO 27001:2022 | 93 | Enterprise ISMS |
| SOC 2 Type II | 74 | SaaS (US market) |
| PCI-DSS v4.0 | 97 | Payment processing |
| HIPAA | 67 | Health data (US) |

### Quality — Web (Certix)

369 rules across 5 profiles (DEV, UX, CONT, OPS, JUR). Two modes: targeted validation (`opo`, before merge) or exhaustive audit (`audit_certix`).

Certix is KARUKIA Solutions' own web quality referential, built for modern web applications.

### Offensive (Viper)

BRIGADE methodology with 16 specialized agents. CVSS v4.0 scoring, MITRE ATT&CK mapping, realistic attack narratives. Web, cloud, and healthcare-specific testing.

### TypeScript Quality

118 checkpoints across 7 categories: type safety, strict configuration, generics, async patterns, modules, errors, and measurable metrics. Grade A-D scoring.

### CSS / Design System

55 checkpoints for maintainability, accessibility, and design system consistency.

### Architecture

70 checkpoints for module structure, coupling/complexity analysis, and architectural layering.

### Test Coverage

68 checkpoints for frontend and backend test inventory and quality sampling.

### Performance

90 checkpoints across frontend performance, backend optimization, and build/bundle analysis.

### Technical Debt

55 checkpoints for dead code detection, dependency health, and code smell identification.

### Expert HDS / ISO 27001

200+ checkpoints across 8 specialized domains for certification readiness: cryptography, audit trail, access control, data classification, multi-tenancy, resilience, vulnerability management, and network security.

### Global Scan (`karukia_scan`)

Meta-orchestrator that runs all 11 dimensions in parallel — 1800+ total checkpoints. Produces a unified scorecard, deduplicates findings, and prioritizes remediation.

---

## 5. The Orchestrator (Auto)

The `auto` tool is the main entry point. The user describes their request in natural language, and the orchestrator routes to the right skill chain automatically.

### Routing table

| Request type | Skill chain |
|-------------|-------------|
| Frontend feature | Jeffrey → Neo → Opo |
| Backend feature | Jeffrey → Neo |
| Security audit | Neo |
| Pentest | Viper |
| Full audit | karukia_scan |
| TypeScript review | ts_quality |
| Architecture review | archi |
| Performance audit | perf |
| Risk analysis | ebios_rm_audit |
| Certification prep | audit_expert_hds |
| Due diligence / code review | deep_review |

---

## 6. Memory System

KARUKIA maintains a structured memory across sessions:

```
karukia/
├── config/
│   ├── security-scope.md         — Active frameworks and constraints
│   └── coverage-scopes.json      — Coverage globs per skill (generated by install)
├── memory/
│   ├── sessions/                 — One directory per task session
│   ├── coverage/                 — Coverage manifests (one per skill)
│   ├── knowledge/                — Lessons learned and reusable patterns
│   └── references/               — Hardening plans
└── trackers/
    └── KARUKIA-TRACKER.json      — Unified chantier tracker (all dimensions)
```

The `KARUKIA-TRACKER.json` file tracks improvement chantiers (tasks) across all 11 dimensions: security, quality, TypeScript, CSS, architecture, tests, performance, and debt. Each skill reads and updates its section automatically.

### Inter-skill communication

Skills communicate via `context.json` in the session directory. When Jeffrey implements a feature, it writes context that Neo reads for security validation. When Neo rejects, Jeffrey reads the rejection reason and fixes. This enables the validation chain: **Jeffrey -> Neo -> Opo** with automatic rejection loop (max 3 iterations).

---

## 7. Coverage Tracking (New in v3.1)

Traditional AI audits are one-shot: the AI scans whatever fits in its context window and stops. If your codebase has 300 files and the AI analyzed 80, you have no way to know which 220 were missed — or to resume where it left off.

KARUKIA v3.1 solves this with **persistent coverage tracking** across sessions.

### How it works

Every audit skill writes a coverage manifest at the end of each scan:

```
karukia/memory/coverage/{skill}-latest.json
```

The manifest records:
- Which files were analyzed (with timestamps)
- Which files were skipped (remaining scope)
- Cumulative coverage percentage
- Findings count by severity (critical, high, medium, low)
- Git branch context

### Iterative scanning

```
Scan 1: Analyze ~40% of in-scope files         -> manifest: 40% coverage
Scan 2: Read manifest, skip analyzed files      -> manifest: 80% coverage
Scan 3: Pick up remaining files                 -> manifest: 100% COMPLETE
```

After a complete scan, KARUKIA detects files modified since the last run (via git) and only re-analyzes those — no wasted effort.

### Scope resolution

Coverage scopes are resolved in order:
1. **Project-specific config** (`karukia/config/coverage-scopes.json`) — generated by `install`, contains globs tailored to the project
2. **Skill defaults** — generic globs (e.g., `**/*.ts` for TypeScript audit, `**/*.css` for CSS audit)

### What this means for teams

A development team running `karukia neo` weekly will reach 100% codebase coverage within 2-3 sessions. The coverage manifest provides auditable proof of what was checked and when — exactly what compliance frameworks require.

```
--- COVERAGE neo ---
Scope total     : 120 files
This scan       : 48 files analyzed
Cumulative      : 96 / 120 (80%)
Status          : PARTIAL

Remaining — next scan starts with:
  - src/auth/session.ts
  - src/api/handlers/patient.ts
  - ... (24 more)
---
```

---

## 8. Standardized 6-Step Workflow (New in v3.1)

All audit skills follow the same structured workflow. This is not an internal implementation detail — the workflow is injected into the prompt and enforced by GUARD.

```
Step 0   : PREPARATION        — Create session, load references and patterns
Step 0.5 : COVERAGE LOADING   — Read previous manifest, prioritize unscanned files
Step 1   : EXPLORATION         — Multi-agent parallel scanning
Step 2   : ANALYSIS            — Synthesize discoveries, plan actions
Step 3   : EXECUTION           — Execute plan, update progress after each action
Step 4   : VALIDATION          — Lint, build, test. Fix ALL issues before closure
Step 4.5 : COVERAGE WRITE      — Write coverage manifest for next session
Step 5   : CLOSURE             — Finalize session, update trackers and knowledge base
```

### Multi-agent exploration

During Step 1, skills dispatch parallel exploration agents, each covering a specific scope:

```
Agent 1 : Explore src/api/       ─┐
Agent 2 : Explore src/auth/       ├── In parallel
Agent 3 : Explore src/services/  ─┘
          Consolidation of results
```

Each agent returns a list of `files_analyzed` in its report. The main context consolidates all lists for coverage tracking.

### Rule of 2 actions

After every 2 read/search operations, findings MUST be written to `findings.md`. This guarantees that even if a session is interrupted (context limit, timeout, user abort), the work done so far is preserved.

### 3-attempt protocol

```
Attempt 1 : Diagnose and fix (root cause)
Attempt 2 : Alternative approach (NEVER repeat the same action)
Attempt 3 : Rethink globally (question assumptions)
After 3   : Escalate to user
```

### Knowledge persistence

At the end of every session, the skill checks for learnings to save:
- `karukia/memory/knowledge/lessons.md` — Errors avoided, gotchas discovered
- `karukia/memory/knowledge/patterns.md` — Reusable code patterns (used 2+ times)

This means the methodology learns from your codebase over time. The second audit is smarter than the first.

---

## 9. Deep Review — 12-Axis Due Diligence (New in v3.1)

The `deep_review` tool is a Staff Engineer-level technical review of an entire codebase. It was designed for three scenarios:

1. **Pre-investment due diligence** — An investor needs to know if the code is worth betting on
2. **CTO takeover** — A new technical leader needs a complete picture in one session
3. **Major refactor planning** — The team needs to know where the real problems are

### The 12 axes

Deep Review deploys a brigade of 6 parallel agents, each covering 2 axes:

| Agent | Axes | What it evaluates |
|-------|------|-------------------|
| Agent 1 | Code Quality + AI Patterns | Functions >100 lines, copy-paste, `any` casts, over-engineering, dead code from AI generation |
| Agent 2 | Architecture + Maintainability | Separation of concerns, circular deps, consistent patterns, magic numbers, bus factor |
| Agent 3 | Scalability + Costs | Hot spots, cold starts, O(n) on tenants, cost-per-tenant modeling |
| Agent 4 | Security + Regulatory | Unauthenticated data paths, frontend secrets, GDPR flows, retention policies |
| Agent 5 | Resilience + Tests | Circuit breakers, graceful degradation, test coverage quality (not just %) |
| Agent 6 | DX + Frontend Perf | Onboarding time, npm scripts, bundle size, lazy loading, re-renders |

### Scorecard

Each axis is scored 0-10 with a letter grade (A+ to F). The global score is out of 120.

```
| # | Axis              | Score /10 | Grade | Red Flags | Strengths |
|---|-------------------|-----------|-------|-----------|-----------|
| 1 | Code Quality      | 7         | B     | 3         | 5         |
| 2 | Architecture      | 8         | A-    | 1         | 4         |
| ...                                                               |

Global Score: 84/120
Global Grade: C

Top 10 Red Flags + Top 10 Strengths + Action Plan (P0/P1/P2)
```

The final verdict answers one question: **"Does this code look like it was written by an expert human, or by an AI?"**

---

## 10. Multi-AI Platform Support (New in v3.1)

KARUKIA is the first MCP methodology server with true multi-AI platform abstraction. All 27 tools accept an optional `client_id` parameter that adapts the entire prompt output.

### Supported platforms

| Platform | `client_id` | Status |
|----------|-------------|--------|
| Claude Code | `"claude"` (default) | Built for, fully tested |
| OpenAI Codex | `"codex"` | Built for, fully tested |
| Any MCP client | `"generic"` | Compatible, not individually tested |

### What adapts

| Aspect | Claude | Codex | Generic |
|--------|--------|-------|---------|
| Sub-agent orchestration | Task API with Sonnet/Opus model hints | Natural language instructions | Natural language |
| Config file generated by `install` | `CLAUDE.md` | `CODEX-PROJECT.md` | `AI-CONFIG.md` |
| Model references in prompts | Opus (primary), Sonnet (exploration) | Generic model names | Generic model names |
| Settings path | `.claude/settings.json` | N/A | N/A |

The platform abstraction is not a thin wrapper — it changes how agents are dispatched, how models are referenced, and how files are organized. The methodology stays identical; the delivery adapts to the client.

---

## 11. For Regulated Industries

### KARUKIA Does Not Replace the Auditor

KARUKIA prepares the evidence dossier so that when the auditor arrives, everything is already structured and traced.

### What KARUKIA Generates

| Auditor's Requirement | What KARUKIA Generates |
|----------------------|------------------------|
| Control evidence | Findings traced with file:line reference |
| Risk mapping | EBIOS RM report (5 ANSSI workshops) |
| Technical security policy | `security-scope.md` generated by `install` |
| Per-framework compliance | Neo reports (HDS, ISO, SOC 2, PCI-DSS…) |
| Certification-level audit | `audit_expert_hds` — 8 domains, 200+ checkpoints |
| Change management | `change_report` — ISO 27001 A.8.32 compliance |
| Documented pentest | Viper report with CVSS v4 + MITRE ATT&CK |

### Built from Real Development

KARUKIA was built while developing a healthcare SaaS application targeting HDS 2.0 and ISO 27001 certification. The methodology was designed to answer a real question: what does it actually take to get certified, from day one of development? The checklists are not theoretical — they reflect what a real auditor asks, point by point.

---

## 12. Use Cases

### SaaS Startup — SOC 2 compliance

1. `karukia install` — detects stack, generates config
2. `karukia neo` with SOC 2 + OWASP — full defensive audit
3. `karukia jeffrey` implements fixes → Neo revalidates
4. Exportable report for the SOC 2 auditor

### Healthcare Application — HDS certification

1. `karukia install` — detects health data, activates HDS 2.0
2. `karukia audit_expert_hds` — 8-domain expert audit
3. `karukia viper` — healthcare-specific pentest
4. `karukia ebios_rm_audit` — formal ANSSI risk analysis
5. Complete documentation for the certification dossier

### Due Diligence — pre-investment or CTO takeover

1. `karukia deep_review` — full 12-axis review in one session
2. Scorecard with A+ to F grades per axis, global score out of 120
3. Top 10 red flags + Top 10 strengths + prioritized action plan (P0/P1/P2)
4. Final verdict: "Does this code look like expert work, or AI-generated output?"

### Development Team — continuous quality

1. `karukia install` on the shared project
2. Developers use `karukia: [task]` daily
3. Every feature goes through Jeffrey → Neo → Opo automatically
4. `karukia_scan` runs periodically for a global health check

---

## 13. Deployment

### Local — Free for personal use (available now)

```bash
npx karukia-mcp
```

Each developer runs the server locally via `npx`. Zero infrastructure, zero cost.

---

## 14. Comparison

| Criteria | Generic AI | KARUKIA v3.1 |
|----------|-----------|---------|
| Methodology | Improvised | 1800+ documented controls |
| Dimensions covered | 1 (whatever you ask) | 12 (security to due diligence) |
| Reproducibility | Variable | Deterministic (same checklists) |
| Traceability | None | Findings with file:line |
| Coverage tracking | None — no idea what was scanned | Persistent manifests, iterative to 100% |
| Multi-session continuity | Starts from zero each time | Coverage + knowledge carry over |
| Multi-AI support | Single platform | Claude, Codex, any MCP client |
| Due diligence | Manual | 12-axis automated review with scoring |
| Compliance | Impossible to prove | Per-framework reports |
| Cost | — | Free for personal use (commercial license for teams) |
| Data sent externally | Depends on provider | None (100% local) |
| Installation | — | `npx karukia-mcp` |

---

## 15. About & License

KARUKIA is developed by **[KARUK IA Solutions](https://karukia.com)**, a B2B SaaS studio specializing in regulated industries (healthcare, finance, pharma), based in Guadeloupe. 🇬🇵

KARUKIA was built while developing a healthcare SaaS application targeting HDS 2.0 and ISO 27001 certification. The methodology was designed to answer a real question: what does it actually take to get certified, from day one of development?

> *Made in Guadeloupe — AI doesn't replace the expert, it frees them.*

KARUKIA MCP is **free for personal and educational use**. No account required.

For commercial use, contact **KARUKIA Solutions** — contact@karukia.com

**npm:** [karukia-mcp](https://www.npmjs.com/package/karukia-mcp)

**GitHub:** [github.com/getkarukia/KARUKIA](https://github.com/getkarukia/KARUKIA)
