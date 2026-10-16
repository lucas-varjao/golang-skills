# Golang skills — full catalog by category

24 skills. Skills marked ⭐️ are recommended for all Go projects. Skills marked ⚙️ can be superseded by a company-specific skill.

---

## Table of Contents

- [Code Quality](#code-quality)
  - [`lucas-varjao/golang-skills@golang-error-handling` ⭐️ ⚙️](#lucas-varjaogolang-skillsgolang-error-handling-%EF%B8%8F-%EF%B8%8F)
  - [`lucas-varjao/golang-skills@golang-safety` ⭐️](#lucas-varjaogolang-skillsgolang-safety-%EF%B8%8F)
  - [`lucas-varjao/golang-skills@golang-security` ⭐️ 🧠](#lucas-varjaogolang-skillsgolang-security-%EF%B8%8F-)
  - [`lucas-varjao/golang-skills@golang-structs-interfaces` ⚙️](#lucas-varjaogolang-skillsgolang-structs-interfaces-%EF%B8%8F)
- [Architecture & Design](#architecture--design)
  - [`lucas-varjao/golang-skills@golang-concurrency` ⚙️](#lucas-varjaogolang-skillsgolang-concurrency-%EF%B8%8F)
  - [`lucas-varjao/golang-skills@golang-context` ⚙️](#lucas-varjaogolang-skillsgolang-context-%EF%B8%8F)
  - [`lucas-varjao/golang-skills@golang-data-structures` ⭐️](#lucas-varjaogolang-skillsgolang-data-structures-%EF%B8%8F)
  - [`lucas-varjao/golang-skills@golang-database` ⭐️ ⚙️](#lucas-varjaogolang-skillsgolang-database-%EF%B8%8F-%EF%B8%8F)
  - [`lucas-varjao/golang-skills@golang-dependency-injection` ⚙️](#lucas-varjaogolang-skillsgolang-dependency-injection-%EF%B8%8F)
  - [`lucas-varjao/golang-skills@golang-design-patterns` ⭐️ ⚙️](#lucas-varjaogolang-skillsgolang-design-patterns-%EF%B8%8F-%EF%B8%8F)
  - [`lucas-varjao/golang-skills@golang-modernize` ⭐️](#lucas-varjaogolang-skillsgolang-modernize-%EF%B8%8F)
  - [`lucas-varjao/golang-skills@golang-refactoring` 🧠](#lucas-varjaogolang-skillsgolang-refactoring-)
- [QA & Performance](#qa--performance)
  - [`lucas-varjao/golang-skills@golang-benchmark` 🧠](#lucas-varjaogolang-skillsgolang-benchmark-)
  - [`lucas-varjao/golang-skills@golang-observability` ⚙️](#lucas-varjaogolang-skillsgolang-observability-%EF%B8%8F)
  - [`lucas-varjao/golang-skills@golang-performance` 🧠](#lucas-varjaogolang-skillsgolang-performance-)
  - [`lucas-varjao/golang-skills@golang-testing` ⭐️ 🧠 ⚙️](#lucas-varjaogolang-skillsgolang-testing-%EF%B8%8F--%EF%B8%8F)
  - [`lucas-varjao/golang-skills@golang-troubleshooting` ⭐️ 🧠](#lucas-varjaogolang-skillsgolang-troubleshooting-%EF%B8%8F-)
- [Project Setup](#project-setup)
  - [`lucas-varjao/golang-skills@golang-cli`](#lucas-varjaogolang-skillsgolang-cli)
  - [`lucas-varjao/golang-skills@golang-continuous-integration`](#lucas-varjaogolang-skillsgolang-continuous-integration)
  - [`lucas-varjao/golang-skills@golang-dependency-management`](#lucas-varjaogolang-skillsgolang-dependency-management)
  - [`lucas-varjao/golang-skills@golang-popular-libraries`](#lucas-varjaogolang-skillsgolang-popular-libraries)
- [APIs](#apis)
  - [`lucas-varjao/golang-skills@golang-swagger`](#lucas-varjaogolang-skillsgolang-swagger)
- [Frameworks](#frameworks)
  - [`lucas-varjao/golang-skills@golang-spf13-cobra`](#lucas-varjaogolang-skillsgolang-spf13-cobra)
  - [`lucas-varjao/golang-skills@golang-spf13-viper`](#lucas-varjaogolang-skillsgolang-spf13-viper)

## Code Quality

### `lucas-varjao/golang-skills@golang-error-handling` ⭐️ ⚙️

Golang error handling best practices — error creation, wrapping with fmt.Errorf and errors.Is/As, sentinel errors, custom error types, panic recovery.

Use when: writing or reviewing error propagation, wrapping, logging, or recovery patterns. For preventing panics → `golang-safety`.

---

### `lucas-varjao/golang-skills@golang-safety` ⭐️

Defensive Golang coding — prevents panics, silent data corruption, and runtime bugs. Nil safety, append aliasing, map concurrent access, float comparison, zero-value design, numeric overflow.

Use when: writing or reviewing code that could silently produce wrong results or panic. Not for external threats (→ `golang-security`) or error handling idioms (→ `golang-error-handling`).

---

### `lucas-varjao/golang-skills@golang-security` ⭐️ 🧠

Golang security best practices — injection prevention (SQL, command, XSS), cryptography, filesystem/network safety, secrets management, cookie security, tool configuration. Audit and review modes.

Use when: auditing a codebase for vulnerabilities, writing security-sensitive code, or reviewing auth/crypto/secrets handling. Not for runtime correctness bugs (→ `golang-safety`).

---

### `lucas-varjao/golang-skills@golang-structs-interfaces` ⚙️

Golang struct and interface design — composition, embedding, type assertions, interface segregation, struct tags (JSON/YAML/DB), pointer vs value receivers.

Use when: designing types, choosing between value vs pointer receivers, writing struct tags, or working with interface hierarchies. For architectural patterns that use interfaces → `golang-design-patterns`.

---

## Architecture & Design

### `lucas-varjao/golang-skills@golang-concurrency` ⚙️

Golang concurrency patterns — goroutines, channels, sync primitives, context cancellation, worker pools, fan-out/fan-in, pipelines, errgroup.

Use when: writing concurrent code, coordinating goroutines, or reviewing for race conditions. For context propagation specifically → `golang-context`. When cancelling goroutines via context — load both.

---

### `lucas-varjao/golang-skills@golang-context` ⚙️

Idiomatic context.Context usage — creation, cancellation, timeouts, values, propagation patterns, WithoutCancel, common anti-patterns.

Use when: propagating deadlines and cancellation signals, or passing request-scoped values. Not for code that merely accepts ctx as first parameter.

---

### `lucas-varjao/golang-skills@golang-data-structures` ⭐️

Golang data structures internals and usage — slices (capacity growth, append aliasing), maps, channels, sync primitives, container/\*, generic collections, and when to use each.

Use when: choosing a data structure, understanding slice/map performance characteristics, or using container/list, container/heap, or ring.

---

### `lucas-varjao/golang-skills@golang-database` ⭐️ ⚙️

Golang database access patterns — parameter binding, connection pooling, transactions, migrations, sqlboiler/sqlc code generation, query builders.

Use when: writing SQL queries, designing repository patterns, or configuring database connections. For security aspects of queries (injection) → also consult `golang-security`.

---

### `lucas-varjao/golang-skills@golang-dependency-injection` ⚙️

Dependency injection patterns in Golang — constructor injection, interface-based DI, wire/dig/fx comparison, and when DI is worth the complexity.

Use when: deciding whether to use DI, designing constructor signatures, or comparing DI libraries.

---

### `lucas-varjao/golang-skills@golang-design-patterns` ⭐️ ⚙️

Idiomatic Golang design patterns — functional options, constructors, builder pattern, middleware chains, circuit breaker, and architecture guides.

Use when: choosing architectural patterns, designing APIs, or implementing resilience patterns. For type-level design (embedding, receivers) → `golang-structs-interfaces`.

---

### `lucas-varjao/golang-skills@golang-modernize` ⭐️

Modernize Golang code using recent language features — range-over-int, min/max builtins, iterators, slices/maps/cmp/slog stdlib packages, testing patterns (t.Context, b.Loop, synctest), and tooling upgrades.

Use when: upgrading a codebase to a newer Go version or replacing pre-generics patterns.

---

### `lucas-varjao/golang-skills@golang-refactoring` 🧠

Safe, at-scale restructuring of existing Go code — coverage-adaptive safety net, behavior-preserving transforms (gopls Rename/Extract, `gofmt -r`, `gopatch`), the Fowler catalog mapped to Go, breaking import cycles, and small stacked PRs.

Use when: a function or type has grown too large, a code smell blocks a feature, or a rename/extract/move has to land without breaking callers. For newer-idiom rewrites → `golang-modernize`.

---

## QA & Performance

### `lucas-varjao/golang-skills@golang-benchmark` 🧠

Golang benchmarking, profiling, and performance measurement — pprof, trace, CPU/memory/block profiles, flame graphs, benchstat, CI regression detection, continuous profiling.

Use when: measuring performance, capturing profiles, comparing benchmark runs, or setting up CI regression detection. For applying optimization patterns → `golang-performance`. For debugging a crash → `golang-troubleshooting`.

---

### `lucas-varjao/golang-skills@golang-observability` ⚙️

Golang production observability — structured logging (slog), Prometheus metrics, OpenTelemetry tracing, pprof profiling endpoints, alerting, Grafana dashboards.

Use when: instrumenting a service for production monitoring. Not for temporary deep-dive investigation (→ `golang-benchmark`, `golang-performance`).

---

### `lucas-varjao/golang-skills@golang-performance` 🧠

Golang performance optimization — allocation reduction, CPU efficiency, memory layout, GC tuning, pooling, caching, hot-path optimization.

Use when: applying optimization patterns after profiling. Not for measurement methodology (→ `golang-benchmark`) or debugging workflow (→ `golang-troubleshooting`).

---

### `lucas-varjao/golang-skills@golang-testing` ⭐️ 🧠 ⚙️

Production-ready Golang tests — table-driven tests, fuzzing, fixtures, goroutine leak detection (goleak), snapshot testing, code coverage, integration tests, parallel tests.

Use when: writing or reviewing tests. For measurement methodology → `golang-benchmark`.

---

### `lucas-varjao/golang-skills@golang-troubleshooting` ⭐️ 🧠

Systematic Golang debugging — common pitfalls, test-driven debugging, pprof capture, Delve debugger, race detection, GODEBUG tracing, production debugging.

Use when: debugging a panic, unexpected output, or hard-to-reproduce bug. Not for interpreting profiles (→ `golang-benchmark`) or applying optimization patterns (→ `golang-performance`).

---

## Project Setup

### `lucas-varjao/golang-skills@golang-cli`

Golang CLI application development — project layout, exit codes, signal handling, I/O patterns, argument parsing, terminal UX.

Use when: building a CLI tool from scratch. For cobra-specific APIs → `golang-spf13-cobra`. For viper configuration → `golang-spf13-viper`.

---

### `lucas-varjao/golang-skills@golang-continuous-integration`

CI/CD pipeline configuration for Golang projects using GitHub Actions — build, test, lint, and release workflows.

Use when: setting up or improving a CI pipeline for a Go project.

---

### `lucas-varjao/golang-skills@golang-dependency-management`

Golang module dependency strategies — go.mod conventions, versioning, replace directives, tool dependencies, and multi-module workspaces.

Use when: managing go.mod, dealing with replace directives, or structuring a multi-module repo.

---

### `lucas-varjao/golang-skills@golang-popular-libraries`

Curated recommendations for production-ready Golang libraries — when the stdlib is enough vs when to reach for a package.

Use when: choosing a library for a new concern (HTTP, logging, testing, etc.). For deep guidance on a specific library → use the library-specific skill.

---

## APIs

### `lucas-varjao/golang-skills@golang-swagger`

OpenAPI/Swagger docs with swaggo/swag — annotation comments, code generation, framework integrations (gin, echo, fiber, chi), security definitions.

Use when: generating OpenAPI documentation from Go code annotations.

---

## Frameworks

### `lucas-varjao/golang-skills@golang-spf13-cobra`

CLI command trees with spf13/cobra — command hierarchy, RunE hooks, flag management, shell completion, usage templates, testing with SetArgs.

Use when: the codebase imports `github.com/spf13/cobra`. For configuration layering → `golang-spf13-viper`. For general CLI architecture → `golang-cli`.

---

### `lucas-varjao/golang-skills@golang-spf13-viper`

Layered configuration with spf13/viper — flag > env > file > KV > default precedence, BindPFlag, hot reload, test isolation, remote KV integration.

Use when: the codebase imports `github.com/spf13/viper`. For CLI command structure → `golang-spf13-cobra`. For general CLI architecture → `golang-cli`.
