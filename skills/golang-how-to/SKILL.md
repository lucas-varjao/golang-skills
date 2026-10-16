---
name: golang-how-to
description: "Golang skills orchestrator — always active on any Golang coding, review, debug, or setup task. Reads the task context and loads the most relevant skills from lucas-varjao/golang-skills, often multiple at once: writing a gRPC service loads golang-testing + golang-error-handling; debugging a panic loads golang-troubleshooting + golang-safety; auditing security loads golang-security + golang-safety. Also: disambiguates competing clusters when two skills seem to overlap (performance vs benchmark vs troubleshooting, DI cluster, safety vs security), and configures the project's agent-config file (CLAUDE.md, AGENTS.md, GEMINI.md, Cursor rules, or Copilot instructions) to force-trigger skills in a project (/golang-how-to configure)."
user-invocable: true
license: MIT
compatibility: Designed for Claude Code, Codex or similar harness. Requires git.
metadata:
  author: samber
  version: "1.4.2"
  openclaw:
    emoji: "🧭"
    homepage: https://github.com/lucas-varjao/golang-skills
    requires:
      bins:
        - go
        - gopls
    install:
      - kind: go
        package: golang.org/x/tools/gopls@latest
        bins: [gopls]
allowed-tools: Read Edit Write Glob Grep Bash(go:*) Bash(git:*) Agent AskUserQuestion LSP Bash(gopls:*) mcp__gopls__*
---

**Persona:** You are a Go skills orchestrator. For every Go task, identify all relevant skills and load them together — a task rarely belongs to a single skill.

**Modes:**

- **Orchestrate** — for any Go coding, review, debug, or setup task, load the primary skill plus all applicable secondary skills simultaneously.
- **Disambiguate** — when two skills seem to overlap, show the boundary table. See [disambiguation.md](references/disambiguation.md).
- **Configure** — write the always-load directive for `golang-how-to` itself, plus an optional `## Required Go skills` block, to the project's agent-config file (CLAUDE.md, AGENTS.md, or equivalent). Follow [project-config.md](references/project-config.md).

**Questions:** In Configure mode, ask the user through the environment's question tool — never as plain-text prose. One question at a time, wait for the answer. If the environment has no question tool, ask in prose with the same options.

**Dependencies:** `gopls` — `go install golang.org/x/tools/gopls@latest`; Claude Code's built-in `LSP` tool also needs `ENABLE_LSP_TOOL=1` and a Go language server wired.

## Skill loading

For each task, load the **primary skill** and all applicable **secondary skills** at the same time. Do not wait — load them together at the start.

| Intent | Primary | Also load |
| --- | --- | --- |
| Design an API, choose a pattern | `golang-design-patterns` | `golang-structs-interfaces` |
| Handle errors idiomatically | `golang-error-handling` | `golang-safety` (nil-heavy code) |
| Write goroutines, channels, sync | `golang-concurrency` | `golang-context` (if cancellation) |
| Pass deadlines / cancel operations | `golang-context` | `golang-concurrency` (if goroutines) |
| Design structs, embed, use interfaces | `golang-structs-interfaces` | `golang-design-patterns` |
| Database queries and transactions | `golang-database` | `golang-error-handling`, `golang-security` |
| Build a CLI command tree | `golang-spf13-cobra` | `golang-cli`, `golang-spf13-viper` (if config) |
| Layer config from flags/env/file | `golang-spf13-viper` | `golang-spf13-cobra` |
| Write tests | `golang-testing` | — |
| Apply optimization patterns | `golang-performance` | `golang-benchmark` (measure first) |
| Measure with pprof / benchstat | `golang-benchmark` | `golang-performance` (fix), `golang-troubleshooting` (root cause) |
| Debug a panic or unexpected behavior | `golang-troubleshooting` | `golang-safety`, `golang-benchmark` (if perf-related) |
| Monitor in production | `golang-observability` | `golang-performance` (if SLO breach) |
| Audit security vulnerabilities | `golang-security` | `golang-safety` |
| Refactor or restructure existing code | `golang-refactoring` | — |
| Set up CI/CD pipeline | `golang-continuous-integration` | `golang-security` |
| Choose a library | `golang-popular-libraries` | relevant library-specific skill |
| Adopt new Go language features | `golang-modernize` | — |
| Use dependency injection | `golang-dependency-injection` | — |

All skill identifiers above are short forms of `lucas-varjao/golang-skills@<name>`.

## Categories at a glance

Full catalog with "use when" hooks: [by-category.md](references/by-category.md)

| Category | Skills |
| --- | --- |
| Code Quality | `golang-error-handling` `golang-safety` `golang-security` `golang-structs-interfaces` |
| Architecture & Design | `golang-concurrency` `golang-context` `golang-data-structures` `golang-database` `golang-dependency-injection` `golang-design-patterns` `golang-modernize` `golang-refactoring` |
| QA & Performance | `golang-benchmark` `golang-observability` `golang-performance` `golang-testing` `golang-troubleshooting` |
| Project Setup | `golang-cli` `golang-continuous-integration` `golang-dependency-management` `golang-popular-libraries` |
| APIs | `golang-swagger` |
| Frameworks | `golang-spf13-cobra` `golang-spf13-viper` |

## Competing clusters — boundary lines

Full boundary tables with routing examples: [disambiguation.md](references/disambiguation.md)

Key clusters and their owners:

- **Performance**: `golang-performance` (optimization patterns) · `golang-benchmark` (measurement) · `golang-troubleshooting` (root cause) · `golang-observability` (always-on production)
- **DI**: `golang-dependency-injection` (concepts/decision)
- **Errors**: `golang-error-handling` (idioms) · `golang-safety` (prevent panics)
- **CLI**: `golang-cli` (architecture) · `golang-spf13-cobra` (command tree) · `golang-spf13-viper` (config layering)
- **Package lookup**: `golang-popular-libraries` (which library to adopt) · `golang-dependency-management` (manage go.mod) · `golang-security` (whole-tree CVE scan)
- **Gap — type vs arch**: `golang-structs-interfaces` (type design) vs `golang-design-patterns` (architectural patterns)
- **Gap — goroutine vs cancel**: `golang-concurrency` + `golang-context` — load both when cancelling goroutines via context
- **Gap — correctness vs threat**: `golang-safety` (internal bugs) vs `golang-security` (external threats)
- **Gap — process vs target rules**: `golang-refactoring` (the safe, staged, at-scale _process_ of changing existing code — planning, ordering, gopls-driven mechanics, staged PRs) vs `golang-design-patterns`/`golang-modernize` (what the resulting code should look like) — load `golang-refactoring` alongside whichever of these owns the target shape

## Configure mode

Write an always-load directive for `golang-how-to` itself to the project's agent-config file (CLAUDE.md, AGENTS.md, GEMINI.md, Cursor rules, or Copilot instructions — whichever the project's harness reads), and optionally force-trigger specific secondary skills too.

Running `/golang-how-to configure` writes the always-load directive if missing, and additionally lets the user confirm a `## Required Go skills` block for skills that must always apply beyond routing. Follow [project-config.md](references/project-config.md).

---

This skill is not exhaustive. Refer to individual skill files and the official Go documentation for detailed guidance.

If you encounter a bug or unexpected behavior in this skill plugin, open an issue at <https://github.com/lucas-varjao/golang-skills/issues>.
