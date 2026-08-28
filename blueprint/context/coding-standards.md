# Coding Standards

These standards reflect the current `llm-wiki-compiler` codebase, the original
repository guidance preserved in `AGENTS - Copy.md`, and the constraints of this
personal fork. Preserve established upstream architecture unless an approved
feature spec requires a focused extension.

## Architecture and Scope

- Keep the Node.js, TypeScript, ESM, filesystem-backed, CLI-first architecture.
- Treat the CLP/profile system, compiler, provider abstractions, source hashing,
  provenance model, incremental processing, SDK, MCP server, and lightweight
  viewer as intentional upstream design.
- Keep structured state deterministic, Git-friendly, and inspectable. Do not add
  a canonical database, hosted service, account system, telemetry, or frontend
  framework without an explicitly approved architectural change.
- Protect upstream synchronization. Prefer custom work in this order:
  1. profile or configuration;
  2. isolated new module;
  3. small extension point in an existing upstream module;
  4. invasive upstream-core modification only when genuinely necessary.
- Avoid moving upstream files, renaming public APIs, broad refactors, duplicated
  upstream capabilities, and domain-specific branches in generic core modules.
- Keep reusable provenance and media infrastructure domain-neutral. Social
  semantics belong in profiles or isolated social modules.
- Read the relevant implementation before making claims or proposing changes.
  Never speculate about code that has not been inspected.
- Keep every change as simple and localized as the approved requirement allows.

## TypeScript and Runtime

- Target Node.js 24 or newer and emit ESM for `node24`.
- Keep TypeScript strict mode enabled.
- Use `.js` extensions for local runtime imports from TypeScript source.
- Avoid `any`. Use precise types or `unknown` with explicit narrowing.
- Prefer type inference when the type is obvious and explicit types at public
  boundaries, data contracts, and non-obvious return paths.
- Use `satisfies` when it verifies a shape without widening useful literals.
- Keep CLI-only registration out of the public SDK entry point.
- Preserve the published CLI and SDK boundaries unless the feature explicitly
  changes their contract.

## File Organization

- Production code lives under `src/`, grouped by capability such as `compiler/`,
  `profile/`, `workflows/`, `viewer/`, `mcp/`, `sdk/`, and `commands/`.
- Commander registration belongs in `src/cli.ts` or focused modules under
  `src/cli/`; command behavior belongs under `src/commands/`.
- Public SDK exports are curated through `src/index.ts`.
- Viewer server code belongs under `src/viewer/`; browser assets remain plain
  JavaScript and CSS under `src/viewer/assets/`.
- Tests live under `test/`, with reusable helpers under `test/fixtures/` or a
  focused test subdirectory.
- TypeScript, JavaScript, Python, shell, and SQL files must stay below 400 lines
  excluding comments. Refactor when a file exceeds the limit.
- Test files must stay below 400 lines excluding comments. Split them by behavior
  or concern when they exceed the limit.
- Markdown, MDX, YAML, JSON, TOML, and other prose or configuration files are
  exempt from the 400-line limit when their content genuinely requires it.

## Functions and Naming

- Functions must stay below 40 lines excluding comments and `catch` or `finally`
  blocks. Extract focused helpers when they exceed the limit.
- Individual tests must stay below 40 lines under the same counting rule.
- Give each function one responsibility.
- Use names that reveal purpose and domain meaning.
- Use camelCase for functions and variables, PascalCase for types and classes,
  and SCREAMING_SNAKE_CASE for module-level constants.
- Use descriptive boolean names that state a condition, such as `isTrusted` or
  `hasPendingWrites`.
- Replace meaningful literals and thresholds with named constants.
- Limit function arguments. Group related inputs into an options object when a
  signature becomes difficult to understand.
- Keep code DRY when duplicated behavior represents one contract. Do not create
  a premature abstraction for superficially similar code with different rules.
- Prefer guard clauses and focused helpers over deep nesting.

## Data, Provenance, and I/O

- Preserve the file-backed project model: Markdown and YAML frontmatter for
  knowledge, JSON or JSONL for structured state, and explicit manifests for
  derived artifacts.
- Keep serialized output stable and deterministic so Git diffs remain useful.
- Preserve stable IDs, source hashes, provenance, and version metadata across
  transformations.
- Treat LLM output and imported data as untrusted input. Validate before writing.
- Exact quotations must come from validated source spans and deterministic source
  reads, never from regenerated model text.
- Use existing path-confinement, private-directory, lock, journal, and atomic-write
  utilities instead of creating weaker local alternatives.
- Fail closed when path safety, profile validity, provenance, gate requirements,
  or write preconditions cannot be established.
- Distinguish user-owned files from temporary or generated files. Never delete a
  user-owned source as part of cleanup.
- Give destructive cleanup a useful dry-run and explain exact targets before the
  mutating path runs.
- Keep binary media outside Git. Git stores stable IDs, hashes, timestamps,
  storage references, relationships, and provenance metadata.

## Error Handling

- Catch errors at process and protocol boundaries, not indiscriminately inside
  core logic.
- CLI failures should produce concise user-facing messages and meaningful exit
  codes without leaking secrets.
- SDK and MCP surfaces should preserve structured error contracts where one
  already exists.
- Do not swallow exceptions or silently weaken a failed safety check.
- Include enough context in an error to identify the failed operation without
  exposing credentials, raw private content, or unsafe absolute paths.
- Preserve transactional and recovery behavior for multi-file writes.

## Viewer and Styling

- Preserve the lightweight server-rendered viewer and plain browser JavaScript.
  Do not introduce a frontend framework for ordinary viewer work.
- Reuse the CSS token system in `src/viewer/assets/viewer-tokens.css`.
- Keep theme behavior, keyboard access, focus visibility, reduced-motion support,
  semantic markup, and security headers intact.
- Put stable browser behavior under the existing Vitest/jsdom or live-server
  harness when proportionate. Do not silently add a separate browser runner.
- Treat visual changes as requiring direct viewer evidence in addition to build
  and test results.

## Testing

Vitest is configured and tests are a required gate for logic-bearing work.

- Add focused regression coverage with every behavior or logic change.
- Test externally visible behavior and safety contracts rather than private
  implementation details.
- Keep tests deterministic and independent of real provider credentials,
  external networks, wall-clock timing, and the user's filesystem.
- Mock LLM/provider boundaries with Vitest spies or existing fixtures.
- Use temporary project roots and existing fixture helpers for filesystem tests.
- Use subprocess integration tests for CLI, packaging, and live viewer behavior
  when an in-process test would miss the contract.
- Preserve security regression coverage for path confinement, symlinks, host and
  origin validation, untrusted imports, and destructive operations.
- Use golden fixtures only for intentional stable parity contracts, and review
  golden changes as API changes.
- Keep shared test setup in focused fixtures rather than repeating large setup
  blocks across test files.
- The normal suite is `npm test`. Packaging coverage excluded from that suite is
  run with `npm run test:pack` when package output changes.
- An empty or skipped suite is not evidence that a change works.

## Required Verification

Before committing work or considering a task complete, run all applicable checks
and resolve every failure:

1. Typecheck: `npx tsc --noEmit`
2. Build: `npm run build`
3. Tests: `npm test`
4. Codebase health: `npm run fallow:ci`

`npm run fallow:ci` is the repository-owned, CI-aligned Fallow command. A task is
not complete while Fallow reports unresolved dead code, duplication, complexity,
or health issues.

Run `npm run release:check-docs` when release documentation or version-facing
content changes. Run `npm run release:check-docs:current` and the package dry-run
for release preparation. There is currently no single combined `Verify` command;
the four required commands above are the completion gate.

## Comments and Documentation

- Include a substantial file-level JSDoc comment in TypeScript and JavaScript
  files that explains the file's purpose, boundary, and non-obvious guarantees.
- Use Google-style module and public API docstrings in Python files.
- Document public APIs and exported functions. Use JSDoc for functions, with
  detail proportional to the behavior and risks involved.
- Comment the why, not a line-by-line description of what the code does.
- Explain complex invariants, security decisions, compatibility workarounds,
  format contracts, and intentionally surprising behavior.
- Keep comments accurate when behavior changes.
- Prefer self-explanatory names and focused functions over narrative comments.
- Do not leave commented-out code, decorative section banners, or stale TODOs.

## Writing

- Be concise, specific, and grounded in inspected code or approved requirements.
- Use plain language for user-facing messages and documentation.
- Use backticks for commands, identifiers, and file paths.
- Use lists for enumerations and tables only when comparing the same fields.
- Do not use em dashes, en dashes, or the ellipsis character in generated prose,
  comments, documentation, specs, or commit messages. Rephrase or use a hyphen.
- Do not add AI attribution to source files, documentation, or commit messages.
