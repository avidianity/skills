---
name: fallow
description: Use Fallow CLI for JavaScript/TypeScript static codebase intelligence: dead code, unused exports, unused dependencies, duplicate code, complexity hotspots, maintainability issues, refactor targets, and Fallow CI/config/debugging.
---

# Fallow CLI Skill

Use this skill for JavaScript and TypeScript repos when asked to run or interpret Fallow, find dead code, unused exports, unused types, unused dependencies, duplicated code, complexity hotspots, maintainability issues, cleanup targets, or refactor targets.

## Mode

Use Fallow's free static-intelligence layer only.

Do not run:

```bash
fallow coverage setup
fallow license activate
fallow health --runtime-coverage ...
```

Static findings are evidence, not proof.

"Unused" means not referenced in the static graph Fallow saw.

"Unreachable" means not reachable from known static entry points.

Static analysis may miss runtime usage through:

- Framework magic
- Dynamic imports
- String-based dispatch
- Plugin registries
- Reflection
- Public package APIs
- Scripts
- Config files
- Generated code
- External consumers

Never delete or broadly refactor based on Fallow alone.

## First Checks

Before running Fallow:

1. Work from the project root.
2. Check for `package.json`, workspace config, `.git`, and existing Fallow config.
3. Check git status so user changes are not mixed with cleanup.
4. Identify likely verification commands such as typecheck, test, lint, or build.

Look for existing config files:

```bash
ls -a
```

Common Fallow config files:

```text
.fallowrc.json
.fallowrc.jsonc
fallow.toml
.fallow.toml
```

Check git status:

```bash
git status --short
```

## Main Commands

Prefer JSON output for agent-readable results:

```bash
npx fallow --format json --summary
npx fallow dead-code --format json --summary
npx fallow dupes --format json --summary
npx fallow health --format json --summary
```

Bare Fallow runs the standard static analyses:

```bash
npx fallow
```

This includes:

- `dead-code`
- `dupes`
- `health`

## Dead Code Commands

Use `dead-code` for cleanup candidates:

```bash
npx fallow dead-code
npx fallow dead-code --format json --summary
npx fallow dead-code --production
npx fallow dead-code --changed-since main
npx fallow dead-code --file src/foo.ts
```

Use trace commands for unclear findings:

```bash
npx fallow dead-code --trace src/utils.ts:formatDate
npx fallow dead-code --trace-file src/utils.ts
npx fallow dead-code --trace-dependency lodash
```

## Duplicate Code Commands

Use `dupes` for repeated logic:

```bash
npx fallow dupes
npx fallow dupes --top 10
npx fallow dupes --threshold 15
npx fallow dupes --changed-since main
```

Start with the default mode.

Use semantic mode only when renamed-variable clones are useful:

```bash
npx fallow dupes --mode semantic
```

Expect more false positives from semantic mode.

## Health Commands

Use `health` for complexity and maintainability:

```bash
npx fallow health
npx fallow health --top 10
npx fallow health --complexity
npx fallow health --file-scores
npx fallow health --hotspots
npx fallow health --targets
npx fallow health --changed-since main
npx fallow health --report-only
```

Do not use runtime coverage options in this skill's default mode.

## Fix Commands

Use `fix` only after reviewing candidates.

Dry run first:

```bash
npx fallow fix --dry-run
npx fallow fix --dry-run --format json
```

Apply fixes only when the user asked for cleanup or when the task clearly requires edits:

```bash
npx fallow fix
```

`fallow fix` edits files in place.

It may remove:

- Unused `export` keywords
- Some unused enum members
- Some unused dependencies
- Some unused package manager catalog entries

It does not automatically fix everything.

It does not safely prove that a file, class member, or public API can be deleted.

After applying fixes, run the relevant verification commands.

## Triage Workflow

When asked to run or fix Fallow findings:

1. Run a broad static scan.
2. Summarize findings by category and risk.
3. Pick small, high-confidence cleanup targets.
4. Verify candidates with search, imports, exports, package entry points, config, tests, scripts, and framework conventions.
5. Use trace commands for unclear findings.
6. Apply scoped edits only when corroborated.
7. Run tests, typecheck, lint, or build.
8. Report what changed and what was left as candidate-only.

Prefer small verified fixes over large cleanup commits.

## Confidence Labels

Use these labels when reporting findings:

### Static-confirmed

Directly proven by Fallow's import, export, or module graph.

Example:

```text
This export has no importers inside the static graph.
```

### Likely cleanup

Fallow finding plus corroborating search or config evidence.

Example:

```text
Fallow reports the dependency unused, and search found no references in source, scripts, or config.
```

### Needs confirmation

Static finding affected by possible runtime usage, public API usage, dynamic loading, framework convention, or missing runtime evidence.

Example:

```text
This file looks unused statically, but it may be loaded by route convention or external consumers.
```

### Do not change yet

High blast radius, plausible false positive, or unclear ownership.

Example:

```text
This package export may be part of the public API. Do not remove it without owner confirmation.
```

## Deletion Rules

Before removing an unused export, check:

- Is it exported through package entry points?
- Is it used by external consumers?
- Is it referenced by framework convention?
- Is it used through dynamic lookup, string keys, or reflection?
- Is it referenced in docs, tests, scripts, generated code, or config?
- Is it intentionally public API?

Before deleting an unused file, check:

- Is it an entry point not detected by Fallow?
- Is it loaded dynamically by filename, path, or glob?
- Is it used by build tooling?
- Is it a route, migration, seeder, worker, CLI, or deployment file?
- Is it generated or copied during release?

Before removing a dependency, check:

- Is it used in scripts?
- Is it used in config files?
- Is it used by generated code?
- Is it a peer dependency?
- Is it used by another workspace?
- Does the package manager lockfile need updating?

Before refactoring duplicate or complex code:

- Avoid wide churn.
- Avoid abstracting coincidental similarity.
- Prefer one or two high-confidence targets.
- Use tests to protect behavior before changing shared logic.

## Reporting Format

Use this format when reporting results:

```markdown
Fallow static scan found:

- Dead code: ...
- Dupes: ...
- Health: ...

I fixed:

- ...

I left as candidates:

- ... because static analysis cannot prove runtime or public usage.

Verification:

- ...
```

## CI and Baselines

For advisory CI:

```bash
npx fallow --ci
npx fallow health --report-only
```

For regression-only cleanup:

```bash
npx fallow dead-code --save-regression-baseline
npx fallow dead-code --fail-on-regression --tolerance 2%
```

Prefer regression gates during adoption so existing backlog does not block unrelated work.

## Config

Use `fallow init` only when the user wants repo configuration, or when the first run has clear false positives caused by generated files, missing entry points, or repo-specific ignore rules.

```bash
npx fallow init
```

## Docs

- Quick start: https://docs.fallow.tools/quickstart
- Static vs runtime intelligence: https://docs.fallow.tools/explanations/static-vs-runtime
- CLI reference: https://docs.fallow.tools/cli/dead-code

