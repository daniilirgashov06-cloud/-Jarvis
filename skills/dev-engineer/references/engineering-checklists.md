# Engineering Checklists (security · performance · testing · review · shipping)

Run the relevant checklist before delivering. These are gates, not suggestions.

---

## Security Checklist (every app, every language)
- [ ] **Input validation** at every boundary (API, CLI, file, user). Allow-list, type/length/format checks.
- [ ] **Injection**: parameterize SQL/NoSQL; avoid shell interpolation (`exec` with arg arrays, not string concat); sanitize for XSS; validate paths (no `../` traversal).
- [ ] **Secrets**: none hardcoded, none in logs/errors/reports, none in VCS. From env/secret manager only. `.env` in `.gitignore`, ship `.env.example`.
- [ ] **AuthN/AuthZ**: authenticate at the edge; authorize per-resource (least privilege); no auth bypass "for now".
- [ ] **Password/keys**: strong hashing (bcrypt/argon2/scrypt — never MD5/SHA1 for passwords); secure random for tokens.
- [ ] **Transport**: TLS/HTTPS; no sensitive data in URLs/query strings (logged).
- [ ] **Errors/Logs**: no stack traces or internals leaked to clients; structured logs without PII/secrets.
- [ ] **Dependencies**: minimal; known CVEs checked; lockfile committed; no `curl | sh` installs.
- [ ] **Deserialization**: never `pickle`/`unserialize`/`eval` untrusted data; use safe formats (JSON) + schema validation.
- [ ] **Least privilege**: default-deny, drop capabilities, no `sudo`/root for app runtime.

## Performance & Complexity Checklist
- [ ] Know the algorithmic complexity; avoid accidental O(n²) in hot paths.
- [ ] Index DB queries for real access patterns; avoid N+1 queries (batch/eager-load).
- [ ] Stream/iterate large data; don't load unbounded data into memory.
- [ ] Cache where it measurably helps; make cache invalidation explicit.
- [ ] Concurrency: avoid shared mutable state; use the language's safe primitives; guard against races/deadlocks.
- [ ] Profile before optimizing; optimize the measured hotspot, not the guessed one.
- [ ] Timeouts + cancellation on all external I/O (network, disk, DB). No infinite waits.

## Testing Checklist
- [ ] Happy path, edge cases (empty, null, 0, max, negative, unicode), and error paths covered.
- [ ] Behavior tested, not implementation (tests survive a refactor).
- [ ] Tests are fast, isolated, deterministic (no network/clock/order dependence; inject time/random).
- [ ] Regression test added for every bug fixed.
- [ ] Boundary/contract tests for APIs and module seams.
- [ ] Coverage is meaningful (critical logic), not just a high %.

## Code Review Checklist
- [ ] Correctness: does it do what's required, including edge cases?
- [ ] Error handling: are errors caught at the right level and propagated with context?
- [ ] Design: clear boundaries, single responsibility, no needless abstraction/over-engineering?
- [ ] Naming & readability: would a new teammate understand it in 6 months?
- [ ] Security: passes the security checklist above.
- [ ] Performance: no obvious traps (N+1, unbounded loops, leaks).
- [ ] Tests present and meaningful.
- [ ] Consistent with the codebase's style and the language's conventions.

## Shipping / Definition of Done
- [ ] Builds/compiles cleanly; lint + format pass with zero errors.
- [ ] All tests pass (verified by running them, when the environment allows).
- [ ] README: what it is, how to install, how to run, how to test, config/env needed.
- [ ] Lockfile + `.env.example` committed; `.gitignore` sane.
- [ ] No secrets, no debug leftovers (`console.log`, `print`, commented-out code, dead code).
- [ ] Dependencies minimal and justified.
- [ ] Verification status stated honestly (what was run vs. what was only reviewed).

## Project Skeleton (generic app — adapt to the stack)
```
<project>/
├── README.md              # what/why/install/run/test
├── <manifest>             # package.json | pyproject.toml | go.mod | Cargo.toml | pom.xml
├── <lockfile>             # committed
├── .gitignore
├── .env.example           # placeholders only
├── src/                   # or app/ / cmd/ + internal/ — source
│   ├── main.<ext>         # entrypoint
│   ├── <module>/          # feature/layer packages
│   └── config.<ext>       # config loading (env)
├── tests/                 # unit + integration
└── docs/                  # design notes (optional)
```

## How I Should Deliver Code (recap)
1. Language + filenames + project tree.
2. Complete runnable code (no stubs/ellipses), "why" comments only.
3. Deps + install command + exact run command.
4. Usage example + how to run tests.
5. Design/assumption notes + out-of-scope.
6. Honest verification status (what was executed vs reviewed).
