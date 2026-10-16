# Competing clusters — deep disambiguation

Ten clusters where skills overlap. Each cluster includes a boundary table, concrete routing examples, and notes on gap cases not yet explicit in source skill descriptions.

---

## Table of Contents

- [1. Performance cluster](#1-performance-cluster)
- [2. Dependency injection cluster](#2-dependency-injection-cluster)
- [3. Error handling cluster](#3-error-handling-cluster)
- [4. CLI cluster](#4-cli-cluster)
- [5. Testing cluster](#5-testing-cluster)
- [6. design-patterns vs structs-interfaces](#6-design-patterns-vs-structs-interfaces)
- [7. concurrency vs context](#7-concurrency-vs-context)
- [8. safety vs security](#8-safety-vs-security)
- [9. Package lookup / discovery cluster](#9-package-lookup--discovery-cluster)
- [10. golang-refactoring vs. the target-state rule skills](#10-golang-refactoring-vs-the-target-state-rule-skills)

## 1. Performance cluster

Four skills form a "deep analysis" cluster. `golang-observability` is the always-on counterpart; the other three are activated on demand.

| Skill | Unique territory | Does NOT own |
| --- | --- | --- |
| `lucas-varjao/golang-skills@golang-performance` | Optimization patterns — "if allocation bottleneck → use sync.Pool", "if hot-path → avoid reflection" | Measurement, profile capture, root cause analysis |
| `lucas-varjao/golang-skills@golang-benchmark` | pprof/trace capture, flame graph interpretation, benchstat comparison, CI regression detection | Deciding which optimization to apply |
| `lucas-varjao/golang-skills@golang-troubleshooting` | Debugging workflow: reproduce, bisect, Delve, race detector, GODEBUG, test-driven debugging | Profile interpretation, optimization patterns |
| `lucas-varjao/golang-skills@golang-observability` | Always-on production signals: structured logs, Prometheus counters, OTel traces, alerting | Temporary investigation, benchmark runs |

**Routing examples:**

- "My HTTP handler is slow in production" → start with `golang-observability` (check metrics/traces), then `golang-benchmark` (capture profiles), then `golang-performance` (apply fixes).
- "My benchmark regressed by 20%" → `golang-benchmark` (benchstat comparison, detect root cause via pprof).
- "I need to reduce allocations in the hot path" → `golang-performance` (pooling, struct layout, escape analysis).
- "The process crashes after 10 minutes under load" → `golang-troubleshooting` (race detector, memory leak, Delve attach).

---

## 2. Dependency injection cluster

Start with `golang-dependency-injection` for library selection.

| Skill | Unique territory |
| --- | --- |
| `lucas-varjao/golang-skills@golang-dependency-injection` | Concepts (why DI, manual injection), constructor patterns, library comparison table |

**Routing examples:**

- "Should I use DI in my project?" → `golang-dependency-injection`.

---

## 3. Error handling cluster

| Skill | Unique territory |
| --- | --- |
| `lucas-varjao/golang-skills@golang-error-handling` | Idiomatic error flow: `fmt.Errorf("%w")`, `errors.Is`/`errors.As`, sentinel errors, single-handling rule, `panic`/`recover` |
| `lucas-varjao/golang-skills@golang-safety` | Preventing errors from occurring: nil checks, zero-value design, integer overflow guards, append aliasing |

**Routing examples:**

- "How should I wrap errors so callers can match them?" → `golang-error-handling`.
- "My service panics on nil pointer dereference" → `golang-safety` (defensive coding) AND `golang-troubleshooting` (debugging the existing crash).

---

## 4. CLI cluster

| Skill | Unique territory |
| --- | --- |
| `lucas-varjao/golang-skills@golang-cli` | Exit codes, signal handling (SIGTERM), stdin/stdout/stderr patterns, progress bars, terminal detection |
| `lucas-varjao/golang-skills@golang-spf13-cobra` | `cobra.Command`, `PersistentPreRunE`, `Args` validators, `ValidArgsFunction`, `SetArgs` in tests |
| `lucas-varjao/golang-skills@golang-spf13-viper` | `viper.BindPFlag`, `AutomaticEnv`, `ReadInConfig`, `OnConfigChange`, test isolation with `viper.Reset()` |

**Routing examples:**

- "My CLI should exit with code 2 on bad args" → `golang-cli`.
- "How do I add shell completion to my cobra command?" → `golang-spf13-cobra`.
- "How do I override config values with env vars?" → `golang-spf13-viper`.
- "I'm building a new CLI from scratch" → `golang-cli` (architecture) + `golang-spf13-cobra` (command tree) + `golang-spf13-viper` (config).

---

## 5. Testing cluster

| Skill | Unique territory |
| --- | --- |
| `lucas-varjao/golang-skills@golang-testing` | Test strategy, table-driven patterns, `t.Parallel()`, `testcontainers`, goleak, coverage, fuzz |

**Routing examples:**

- "How do I write a table-driven test in Go?" → `golang-testing`.
- "How do I detect goroutine leaks in tests?" → `golang-testing` (goleak).

---

## 6. design-patterns vs structs-interfaces

These two skills overlap on "how to design Go types". The split is: type-level design vs. architectural use of types.

| Skill | Unique territory | Does NOT own |
| --- | --- | --- |
| `lucas-varjao/golang-skills@golang-structs-interfaces` | Composition over inheritance, embedding, type assertions, struct tag conventions, pointer vs value receivers, interface segregation | How types combine into architectural patterns |
| `lucas-varjao/golang-skills@golang-design-patterns` | Functional options, middleware chains, circuit breaker, graceful shutdown, retry patterns | Low-level type mechanics |

**Overlap zone:** "DI via interfaces" — defining small interfaces is `golang-structs-interfaces`; wiring multiple components together via those interfaces is `golang-design-patterns`.

**Routing examples:**

- "Should my method take a value or pointer receiver?" → `golang-structs-interfaces`.
- "How do I implement a middleware chain for my HTTP handlers?" → `golang-design-patterns`.
- "How do I embed a struct without exposing its methods?" → `golang-structs-interfaces`.
- "How do I implement the functional options pattern?" → `golang-design-patterns`.

> Note: neither skill currently carries a description-level `→ See` disclaimer for this overlap. The boundary above is the intended design, not yet explicit in the source skills.

---

## 7. concurrency vs context

These skills overlap when goroutines are cancelled via context.

| Skill | Unique territory | Does NOT own |
| --- | --- | --- |
| `lucas-varjao/golang-skills@golang-concurrency` | Goroutine lifecycle, channel patterns, `sync.WaitGroup`, `errgroup`, worker pools, fan-out/fan-in, detecting races | Context propagation rules |
| `lucas-varjao/golang-skills@golang-context` | `context.WithCancel`, `context.WithTimeout`, `context.WithValue`, propagation through call chains, `WithoutCancel` | Goroutine coordination patterns |

**Overlap zone:** "cancelling goroutines via context" — load both skills. `golang-context` owns the context API; `golang-concurrency` owns the goroutine coordination.

**Routing examples:**

- "How do I cancel a goroutine from outside?" → both (`golang-context` for `cancel()`, `golang-concurrency` for `select { case <-ctx.Done() }`).
- "How do I fan out N workers and collect results?" → `golang-concurrency`.
- "How do I pass a deadline through multiple layers of function calls?" → `golang-context`.

> Note: no description-level disclaimer currently exists between these two skills. Load both when the task involves both concerns.

---

## 8. safety vs security

Both skills prevent bugs, but with different threat models.

| Skill | Unique territory | Does NOT own |
| --- | --- | --- |
| `lucas-varjao/golang-skills@golang-safety` | Nil panics, integer overflow, slice aliasing via append, concurrent map write, float equality, zero-value design | External attackers, cryptography, secrets |
| `lucas-varjao/golang-skills@golang-security` | SQL/command/LDAP injection, weak crypto (`math/rand`), hardcoded secrets, TLS misconfiguration, SSRF, path traversal | Internal runtime correctness |

**Routing examples:**

- "My service panics with nil pointer dereference" → `golang-safety`.
- "Is this SQL query safe from injection?" → `golang-security`.
- "I'm using `math/rand` to generate a token" → `golang-security` (predictable output; use `crypto/rand`).
- "My slice grows unexpectedly after append" → `golang-safety` (append aliasing).

> Note: no description-level disclaimer currently exists between these two skills. The source descriptions cross-reference in their bodies but not in the YAML frontmatter.

---

## 9. Package lookup / discovery cluster

These three skills all touch "third-party packages", but each owns a different stage: deciding, managing, or remediating.

| Skill | Unique territory | Does NOT own |
| --- | --- | --- |
| `lucas-varjao/golang-skills@golang-popular-libraries` | Recommending a library for a use case; stdlib-vs-third-party judgment | Looking up facts about a specific published package |
| `lucas-varjao/golang-skills@golang-dependency-management` | Editing go.mod: `go get`, upgrading, pinning, `replace`/`exclude`, workspaces | Browsing a package's docs or version history |
| `lucas-varjao/golang-skills@golang-security` | Whole-tree vulnerability scanning with `govulncheck`, remediation across the module | Checking one module's CVEs without scanning the tree |

**Overlap zone:** "is dependency X safe / current?" — `golang-dependency-management` performs the upgrade/pin; `golang-security` scans your own code path for reachable vulnerabilities.

**Routing examples:**

- "Which logging library should I adopt?" → `golang-popular-libraries`.
- "Upgrade github.com/foo/bar to the latest version" → `golang-dependency-management`.
- "Scan my whole module for reachable CVEs" → `golang-security` (`govulncheck`).

---

## 10. golang-refactoring vs. the target-state rule skills

`golang-refactoring` owns the _process_ of changing existing Go code safely at scale — planning, blast-radius mapping, ordering staged PRs, tool-driven mechanics (gopls Rename/Inline, `gofmt -r`, `eg`, `gopatch`), and the human-in-the-loop git model. It does not own what the resulting code should look like — that is split across two existing skills, each answering "refactor toward what?"

| Skill | Unique territory | Does NOT own |
| --- | --- | --- |
| `lucas-varjao/golang-skills@golang-refactoring` | The safe, staged, at-scale process: blast-radius mapping, PR ordering (structural-before-behavioral, conflict-avoidance, dependency), the refactoring-branch git model, tool-driven mechanics, coverage-adaptive safety net | Naming choices, target package layout, target design patterns, idiom adoption — the _shape_ the code should end up in |
| `lucas-varjao/golang-skills@golang-design-patterns` | Target patterns: options structs, consumer-side interfaces, DI | Sequencing a multi-step migration toward one of these patterns as reviewable PRs |
| `lucas-varjao/golang-skills@golang-modernize` | Version-driven idiom adoption (`interface{}`→`any`, `slices`/`maps`) — typically a single mechanical sweep | Multi-step structural refactors that need staged, human-reviewed PRs |

**Overlap zone:** almost every real refactor touches both — the target-rule skill for the _target_, plus `golang-refactoring` for the _how_: blast-radius mapping, the type-alias gradual-repair recipe, and whether this lands as one PR or a staged sequence. Load `golang-refactoring` together with whichever target-rule skill defines the destination shape.

**Routing examples:**

- "This function is too long, break it up" → `golang-refactoring` (Extract Function mechanics, verify behavior preserved).
- "Move this type to a new package without breaking every caller" → `golang-refactoring` (type-alias gradual-repair recipe, ordering).
- "Convert all `interface{}` to `any` across the module" → `golang-modernize` alone is usually sufficient (single mechanical sweep); reach for `golang-refactoring`'s staged-PR flow only if the sweep is large enough to need progressive human review.
- "Plan a multi-week refactor to break up this god package" → `golang-refactoring` as the primary skill (planning gate, ordering, staged PRs), pulling in `golang-design-patterns` for the target shape at each step.
