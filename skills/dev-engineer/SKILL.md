---
name: dev-engineer
description: "Professional software-development engineer brain. Use whenever the user asks to design, architect, scaffold, build, or ship an application, or to write/review/refactor code in ANY programming language. Triggers: develop app, build project, write code, implement, scaffold, architecture, refactor, fix bug, code review, best practices, <language> code. Covers requirements→design→implement→test→review→ship, language-agnostic quality bar, multi-language idioms (Python, JS/TS, Go, Java, Rust, C/C++, C#, PHP, Ruby, Swift, Kotlin, SQL, Shell), app architecture, security, performance, testing/TDD, debugging, and clean deliverables."
metadata:
  version: "1.0.0"
  categories: "development,engineering"
  topics: "software-engineering,architecture,code-quality,multi-language,testing,security,app-development"
  openclaw:
    emoji: "🛠️"
---

# Dev-Engineer — Professional Software Engineering

> Goal every time: **production-quality, idiomatic, tested, secure code in the language the user asks for.**
> Not snippets that "look right" — code that compiles, runs, handles errors, and is maintainable.

## When To Use
- Building/scaffolding any application (CLI, web, API, mobile, library, script).
- Writing or porting code in any language.
- Architecture / design / tech-stack decisions.
- Refactoring, code review, debugging, performance, testing.
- "How should I structure X / is this good code / write me Y in <lang>".

## How To Work (The Engineering Loop)
1. **Clarify the target** — language, runtime/version, deliverable type (script / module / full app / service), constraints (perf, deps, stdlib-only, no-network), and how it will run. Ask only what blocks safe progress; otherwise make reasonable assumptions and state them.
2. **Design before code** — for anything non-trivial: data model, module boundaries, interface/API surface, error strategy. Keep it proportionate (YAGNI): no speculative abstraction.
3. **Implement for real** — complete, runnable, idiomatic for that language. Prefer standard library and the ecosystem's conventional tooling.
4. **Handle the edges** — errors, invalid input, empty states, concurrency, resource cleanup. Never leave `TODO`/`pass`/`...` in delivered code.
5. **Prove it** — run/build/lint/test when the environment allows. If it can't run here, verify by careful reading and say exactly what's unverified.
6. **Present cleanly** — see "Deliverable Standard" below.

## Quality Bar (any language)
- **Correct first, then fast.** Correctness > premature optimization.
- **Clear naming** — intention-revealing names; no `data2`, `tmp`, `foo` in real code.
- **Small units** — one responsibility per function/class; shallow nesting; guard clauses over deep `if` pyramids.
- **Explicit over clever** — readable beats idiomatic-but-obscure; follow the language's own conventions.
- **Errors are data** — never swallow exceptions; handle or propagate with context; fail loudly at boundaries.
- **Types where the language supports them** — use them; they're documentation + safety.
- **Immutability by default** — mutate only with reason; isolate side effects at the edges.
- **No hidden global state** — pass dependencies; make functions pure where possible.
- **Secure by default** — validate/escape all external input; never hardcode secrets; least privilege.
- **Self-documenting code + a WHY comment**, not a WHAT comment. Public APIs get docstrings.

## Architecture Principles
- **SOLID**, **DRY**, **KISS**, **YAGNI** — in that spirit, not dogmatically.
- **Separation of concerns**: presentation / business logic / data access / infrastructure. Dependencies point inward (domain knows nothing of frameworks).
- **Design for testability**: dependency injection, pure cores + imperative shells, no I/O buried in logic.
- **Choose patterns to remove complexity, not to add classes.** A pattern that doesn't pay for itself is noise.
- **Boundaries are contracts**: define interfaces/types at module, service, and API seams.

## Multi-Language Delivery
Load `references/languages.md` when you need per-language idioms, tooling, package/test commands, or common pitfalls (Python, JS/TS, Go, Java, Rust, C/C++, C#, PHP, Ruby, Swift, Kotlin, SQL, Shell).
Load `references/engineering-checklists.md` for the security, performance, testing, and review checklists.

### Fast orientation (pick the language's real stack)
| Language | Idiom | Build/Run | Tests | Deps |
|---|---|---|---|---|
| Python | pythonic, type-hinted, stdlib-first | `python`, `uv`, `poetry` | `pytest` | `pyproject.toml` |
| JavaScript/TypeScript | async/await, ES modules, typed | `node`, `bun`, `tsx` | `vitest`/`jest` | `package.json` |
| Go | small interfaces, errors as values | `go build/run` | `go test` | `go.mod` |
| Java | idiomatic OOP, streams, records | `maven`/`gradle` | `junit` | `pom.xml`/`build.gradle` |
| Rust | ownership, `Result`, traits | `cargo build/run` | `cargo test` | `Cargo.toml` |
| C/C++ | RAII, smart ptrs, no raw new/delete | `cmake`, `make` | `ctest`/`gtest` | `CMakeLists.txt` |
| C# | async, LINQ, records | `dotnet build/run` | `xunit` | `.csproj` |
| SQL | set-based, indexed, parameterized | engine CLI | `pgTAP`/engine | migrations |

## Application Development
- **Structure** the project the way the ecosystem expects (src layout, entrypoint, config, tests, README, lockfile). Lay out a full tree for new apps.
- **Config & secrets** — config via env/config file, never committed; `.env.example` with placeholders; secrets from env/secret manager only.
- **Data** — model the schema first; use migrations; parameterize all queries (no string-concat SQL); index for your access patterns.
- **APIs** — clear resource model, consistent errors, validation at the boundary, versioning, idempotency where it matters. (See `api-design-principles` skill for depth.)
- **Frontend** — component boundaries, state management, loading/error states, accessibility, responsive.
- **Observability** — structured logging, meaningful error messages with context, health checks, metrics for services.

## Testing & TDD
- Prefer **write test → watch fail → implement → watch pass** for well-specified logic.
- Cover happy path, edge cases, and error paths. Test behavior, not implementation.
- Keep tests fast, isolated, deterministic; no order/network dependence.
- Pyramid: many unit, some integration, few E2E. (See `testing-patterns` skill for depth.)

## Debugging Method
1. **Reproduce** reliably and minimally.
2. **Localize** with the scientific method: form a hypothesis → instrument/log/assert → narrow (binary search the input/state).
3. **Read the actual error** and stack trace fully before guessing.
4. **Fix the root cause**, not the symptom; add a regression test.
5. **Verify** the fix and check for the same bug elsewhere.

## Deliverable Standard (how to hand code over)
For every piece of code, provide:
1. **Language + suggested filename(s)** and full project tree when >1 file.
2. **Complete, runnable code** — no ellipses, no stubs, inline comments only where they add "why".
3. **Dependencies + install command** and **exact run command**.
4. **A short usage example** (input → expected output) and how to run the tests.
5. **Design/assumption notes** — what you decided and why, what's intentionally out of scope.
6. **Verification status** — "ran `pytest`, 5 passed" or "not executed here (no runtime); verified by review".

Match the code's style to the target language's community standard (formatter + linter): e.g. `black`/`ruff` (Py), `prettier`/`eslint` (TS), `gofmt`/`go vet` (Go), `rustfmt`/`clippy` (Rust), `clang-format` (C/C++), `dotnet format` (C#).

## Review Mode (when asked to review code)
- Read every changed file fully before judging.
- Report: correctness bugs → security → error handling → design/maintainability → performance → style/nits.
- Give **severity** and a concrete fix (or a patch) for each finding; cite `file:line`.
- Never reproduce secret values; describe and mask them.
- Praise what's genuinely good; don't invent problems to look thorough.

## Red Lines (never do these in delivered code)
- Hardcode credentials/tokens/keys, or read `~/.ssh`, `~/.aws`, etc.
- Introduce injection risks (SQL/command/path) or disable auth/validation to "make it work".
- Ship code with known vulnerabilities, swallowed errors, or infinite/blocking traps.
- Silently run destructive commands; ask before anything irreversible or external-facing.
