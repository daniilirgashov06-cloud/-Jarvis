# Per-Language Idioms, Tooling & Pitfalls

Use this to write **idiomatic** code and to pick the right tooling for the language the user asks for.
Always prefer the ecosystem's standard formatter + linter + test runner and the conventional project layout.

---

## Python
- **Style:** pythonic; `snake_case` functions/vars, `PascalCase` classes, `UPPER` constants. Type-hint all public APIs (`def f(x: int) -> str:`). EAFP over LBYL (`try/except`, not preemptive `if`).
- **Modern:** f-strings, `pathlib`, `dataclasses`/`pydantic`, context managers (`with`), generators for streams, `match` (3.10+), `typing` (`Optional`, `Protocol`, `TypeAlias`).
- **Layout:** `pyproject.toml` (PEP 621), `src/<pkg>/`, `tests/`, `README.md`, lockfile (`uv.lock`/`poetry.lock`). Use `uv` or `poetry` for deps/venv.
- **Run/Build:** `uv run main.py` / `python -m <pkg>`; package with `python -m build`.
- **Tests:** `pytest` (+ `pytest-cov`, `pytest-asyncio`). Name files `test_*.py`.
- **Lint/Format:** `ruff` (lint+format), `mypy` (types), `black` if needed.
- **Async:** `asyncio`, `async/await`, `httpx`/`aiohttp`; don't mix blocking I/O in async paths.
- **Pitfalls:** mutable default args (`def f(x=[])`), late-binding closures in loops, bare `except:`, `import *`, shadowing builtins.

## JavaScript / TypeScript
- **Style:** `camelCase` vars/functions, `PascalCase` types/classes, `UPPER` constants. Prefer `const`; `let` only when reassigning; never `var`. ES modules (`import/export`).
- **Modern:** `async/await` over raw promises, optional chaining `?.`, nullish `??`, destructuring, spread, template literals, `Map`/`Set`.
- **TypeScript:** strict mode (`strict: true`); avoid `any` (use `unknown` + narrowing); model with `type`/`interface`; narrow with type guards; use discriminated unions for state.
- **Layout:** `package.json`, `tsconfig.json`, `src/`, `tests/` (or `__tests__`), lockfile (`package-lock.json`/`pnpm-lock.yaml`/`bun.lockb`). Use `pnpm` or `bun` or `npm`.
- **Run/Build:** `node dist/`, `tsx src/main.ts`, `bun run`, `vite`/`tsc`/`esbuild` to bundle.
- **Tests:** `vitest` (preferred) or `jest`; Playwright for E2E.
- **Lint/Format:** `eslint` + `prettier`.
- **Async pitfalls:** unhandled promise rejections, missing `await`, mixing callback/promise, blocking the event loop, race conditions on shared state.
- **Security:** never `eval`/`innerHTML` unsanitized input; sanitize/escape for XSS; validate on server; parameterize DB queries (use an ORM/query builder).

## Go
- **Style:** `camelCase` (unexported) / `PascalCase` (exported); short names in small scopes. `gofmt` is mandatory.
- **Idioms:** errors as values (`if err != nil { return fmt.Errorf("...: %w", err) }`), small interfaces defined by the consumer, table-driven tests, `defer` for cleanup, composition over inheritance.
- **Layout:** `go.mod`, `cmd/<app>/main.go`, `internal/` (private), `pkg/` (public, optional), `*_test.go` next to code.
- **Run/Build:** `go build ./...`, `go run ./cmd/<app>`, `go vet ./...`.
- **Tests:** `go test ./...`, `-race`, `testify` optional. Benchmarks: `go test -bench`.
- **Pitfalls:** unchecked errors, goroutine leaks (no cancel/context), data races (use `-race`), nil map/slice confusion, over-embedding.

## Java
- **Style:** `camelCase` methods/vars, `PascalCase` classes, `UPPER_SNAKE` constants. Prefer immutability, `record` for DTOs, `Optional` for absent values, streams for collection transforms.
- **Modern:** `var` for obvious types, text blocks, switch expressions, `List.of`, `Map.of`, sealed interfaces (17+).
- **Layout:** Maven `pom.xml` or Gradle `build.gradle(.kts)`; `src/main/java`, `src/test/java`, package by feature.
- **Run/Build:** `mvn package` / `./gradlew build`; run `java -jar target/app.jar`.
- **Tests:** JUnit 5 (+ AssertJ, Mockito). 
- **Pitfalls:** swallowing exceptions, raw types, `==` vs `.equals()`, mutable shared state, resource leaks (use try-with-resources).

## Rust
- **Style:** `snake_case` fns/vars, `PascalCase` types, `SCREAMING` consts. `rustfmt` + `clippy` mandatory.
- **Idioms:** ownership/borrowing; `Result<T, E>` + `?` for errors (`thiserror`/`anyhow`); `Option<T>` for null; iterators over index loops; traits for shared behavior; `&str` over `String` for params.
- **Layout:** `Cargo.toml`, `src/main.rs` (bin) or `src/lib.rs` (lib), `src/` modules, `tests/`, `examples/`, `benches/`.
- **Run/Build:** `cargo build`, `cargo run`, `cargo check` (fast), `cargo clippy`.
- **Tests:** `cargo test`, `#[test]`, integration in `tests/`, `proptest`/`quickcheck` for property tests, `criterion` for benches.
- **Pitfalls:** fighting the borrow checker (restructure, don't `clone()` everywhere), `unwrap()`/`expect()` in library code (return `Result`), `unsafe` without justification, `Rc<RefCell>>` overuse.

## C / C++
- **Style (C++):** RAII, smart pointers (`unique_ptr`/`shared_ptr`) — no naked `new`/`delete`, `auto` where it clarifies, range-for, `nullptr`, `enum class`, `override`, `= delete`. `clang-format` + `clang-tidy`.
- **C:** explicit error codes, check every `malloc`/`fopen`, always free on all paths, no buffer overflows (`snprintf`, bounds-check).
- **Layout:** `CMakeLists.txt`, `include/` (headers), `src/`, `tests/`. C++17/20 baseline.
- **Run/Build:** `cmake -B build && cmake --build build`, or `make`.
- **Tests:** `ctest`, GoogleTest, Catch2. Sanitizers: `-fsanitize=address,undefined`.
- **Pitfalls:** memory leaks, dangling pointers, UB (uninitialized vars, signed overflow, out-of-bounds), missing `virtual` destructor, copy vs move confusion.

## C# / .NET
- **Style:** `PascalCase` types/methods/properties, `camelCase` params, `_camelCase` private fields. `dotnet format`.
- **Modern:** `async/await`, LINQ, records, pattern matching, nullable reference types, `IAsyncEnumerable`, minimal APIs for services.
- **Layout:** `.sln` + `.csproj`, `Program.cs`, layered `Domain/Application/Infrastructure/Api`.
- **Run/Build:** `dotnet build`, `dotnet run`, `dotnet publish`.
- **Tests:** xUnit (+ FluentAssertions, NSubstitute), `dotnet test`.
- **Pitfalls:** blocking `.Result` on async (deadlocks), `IDisposable` leaks (`using`), mutable shared state, exceptions used for control flow.

## PHP
- **Style:** PSR-12; `camelCase` methods, `PascalCase` classes. Composer for deps. Modern: typed properties/returns, `match`, enums, `readonly`, arrow fns.
- **Layout:** Composer `composer.json`, `src/`, `tests/`, `public/index.php`, PSR-4 autoload. Use Laravel/Symfony for apps.
- **Run:** `php -S localhost:8000` (dev) / framework server; `composer install`.
- **Tests:** PHPUnit / Pest.
- **Security:** never trust `$_GET/$_POST`; use prepared statements (PDO/mysqli), CSRF protection, escape output (`htmlspecialchars`), avoid `eval`/`unserialize` on user data.

## Ruby
- **Style:** `snake_case`, idiomatic blocks, `do...end` multi-line, `?`/`!` method suffixes. Bundler (`Gemfile`). Rails for web apps.
- **Run:** `ruby file.rb`, `bundle exec rails s`, `rspec`/`minitest` for tests.
- **Pitfalls:** monkey-patching, `rescue` swallowing errors, `and`/`or` vs `&&`/`||` precedence.

## Swift / Kotlin (mobile & modern)
- **Swift:** value types (`struct`), `guard` for early exits, optionals + `if let`/`guard let`, protocol-oriented, `async/await`. Xcode/SPM; XCTest.
- **Kotlin:** `val` over `var`, null-safety (`?.`, `?:`, `!!` sparingly), data classes, coroutines (`Flow`/`suspend`), sealed classes for state. Gradle; JUnit/`kotest`. Use for Android & backend (Ktor/Spring).

## SQL (any engine)
- Set-based thinking; avoid row-by-row cursors. `JOIN` not correlated subqueries where possible.
- **Always parameterize** queries — never string-concatenate values (SQL injection).
- Schema-first: normalize to 3NF then denormalize for measured performance; add indexes for access patterns; use migrations (Flyway/Alembic/prisma).
- Transactions: keep short; correct isolation level; handle deadlocks. Know the difference between `INNER`/`LEFT` joins, `WHERE` vs `HAVING`, window functions.

## Shell (bash)
- `set -euo pipefail` for scripts; quote all `"$vars"`; prefer `[[ ... ]]`; use `shellcheck`.
- Prefer a real language once logic grows past simple pipes. No unquoted glob/rm on variables.

---

## Universal "pick the stack" defaults (when the user doesn't specify)
- **CLI/script:** Python (`uv`) or Go (single binary) or Rust (perf-critical).
- **Web API/backend:** TypeScript (Node/Bun) or Python (FastAPI) or Go (chi/gin) or Java (Spring) or C# (ASP.NET).
- **Web frontend:** React + TypeScript + Vite (or Next.js for SSR/full-stack).
- **Mobile:** Kotlin (Android) / Swift (iOS) / React Native or Flutter (cross-platform).
- **Data/ML:** Python (pandas, scikit-learn, PyTorch).
- **Systems/perf:** Rust or C++.

State which stack you chose and why; ask if the choice has real consequences.
