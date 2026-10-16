# golang-skills

Agent skills for writing, reviewing and debugging Go code with Claude Code, Codex and similar coding agents.

This repository is a curated, revised copy of [samber/cc-skills-golang](https://github.com/samber/cc-skills-golang) by Samuel Berthe. The content of every skill is his work. I took 25 of the original 46 skills at commit [`8e899e2`](https://github.com/samber/cc-skills-golang/commit/8e899e2), audited them, and fixed what I found. If you want the full set, use the original repository.

## Skills

| Area | Skills |
| --- | --- |
| Routing | `golang-how-to` loads the other skills a task needs |
| Code quality | `golang-error-handling`, `golang-safety`, `golang-security`, `golang-structs-interfaces` |
| Design | `golang-concurrency`, `golang-context`, `golang-data-structures`, `golang-database`, `golang-dependency-injection`, `golang-design-patterns`, `golang-modernize`, `golang-refactoring` |
| Testing and performance | `golang-benchmark`, `golang-observability`, `golang-performance`, `golang-testing`, `golang-troubleshooting` |
| Project setup | `golang-cli`, `golang-continuous-integration`, `golang-dependency-management`, `golang-popular-libraries` |
| Libraries | `golang-spf13-cobra`, `golang-spf13-viper`, `golang-swagger` |

Each skill lives in `skills/<name>/`, with a `SKILL.md` and, for most skills, a `references/` directory the agent reads on demand.

## What changed from the original

The git history keeps each step as its own commit, starting from an unmodified import, so `git log -p` shows every change.

1. **Fewer skills.** I left out code-style, documentation, lint, naming, gopls, pkg-go-dev, project-layout, stay-updated, graphql, grpc, google-wire, uber-dig, uber-fx, the seven samber-* library skills and stretchr-testify. I don't use them, and every skill description costs context in every session. The remaining skills no longer route to, recommend or link any of them. The libraries themselves (samber/oops, testify, gopls and so on) are still mentioned where the content uses them.
2. **Prompt audit for current Claude models.** I ran Claude Code's `prompt-audit` against Claude Opus 5.5. The fixes:
   - 9 skills told the model to "use `ultracode`" to opt into multi-agent Workflows. In Claude Code that opt-in belongs to the user, so the text now says so.
   - "Reason as thoroughly as possible" lines now state why the task needs care. The `ultrathink` keyword stays.
   - Caps-lock pressure and "old API" phrasing were rewritten as plain rules.
3. **Files that contradicted themselves.** Examples:
   - the viper reference said `Unmarshal` can't decode durations and imported `mitchellh/mapstructure`, while viper v1.21 decodes durations by default and uses `go-viper/mapstructure/v2`;
   - a repository mock didn't match its interface;
   - a Prometheus collector declared one metric and emitted another;
   - a heading promised 6 `unsafe.Pointer` patterns and listed 4;
   - `go get -u` was called safe in one paragraph and in need of review in the next.
4. **Technical corrections**, checked against the Go 1.27.1 spec, the runtime sources and the viper v1.21.0 source:
   - Go 1.24+ maps are Swiss tables;
   - Go 1.27 has generic methods;
   - `subtle.ConstantTimeCompare` returns early on a length mismatch;
   - a log sanitizer let `\n` through;
   - a template example used undefined variables;
   - Compose v2 commands.
5. **Namespace.** Cross-references use `lucas-varjao/golang-skills@<skill>`. `metadata.author` still says `samber`.

Not imported: the per-skill `evals/`, the plugin manifests, the CI workflows and the Cursor rule from the original repository.

Known gaps: many performance numbers in the references have no source, and I did not check every "Go 1.27+" claim. Treat them as starting points.

## Install

With the [`skills`](https://github.com/vercel-labs/skills) CLI:

```sh
npx skills add lucas-varjao/golang-skills
```

Or by hand. Clone the repository and link each skill into your agent's skills directory:

```sh
git clone https://github.com/lucas-varjao/golang-skills.git
for s in golang-skills/skills/golang-*; do ln -s "$PWD/$s" ~/.claude/skills/; done
```

With symlinks, `git pull` updates the skills in place.

## License

MIT, same as the original. See [LICENSE](LICENSE). The original copyright notice is kept.
