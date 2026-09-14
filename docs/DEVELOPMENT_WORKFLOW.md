# Development Workflow

## Branching

Create phase branches exactly as listed in `PROJECT_PLAN.md`, based on current `main`.

```powershell
git switch main
git pull --ff-only
git switch -c phase/01-forensics
```

Use focused commits. A phase is not complete merely because code exists; its report
and acceptance evidence are part of the deliverable.

## Start of a Codex session

Ask Codex to:

1. read `AGENTS.md` and the documents named there;
2. identify the active phase from the branch;
3. inspect Git status and preserve unrelated work;
4. state whether planned actions are cheap or heavy;
5. avoid heavy execution unless explicitly authorized.

For Phase 01, give Codex access to the local full-history `EcuadorElectricGrid`
checkout as a read-only evidence path. Do not copy consumer-owned generation logic.

## End of a phase

- satisfy every acceptance criterion;
- update config and architecture if decisions changed;
- write the technical report;
- include exact commands, versions, timings, logs, checksums, and deviations;
- add useful figures under `reports/figures/phase-XX/`;
- run only permitted cheap tests;
- provide Fernando with the manual heavy commands;
- record commands deliberately not run.

## Commit discipline

Do not commit local environments, vendor/tool directories, NetCDF or bulk data,
credentials, secret-bearing logs, or partial artifacts.

Do commit source, config, lock files, tests, small fixtures, reports, reasonable report
figures, provenance schemas, notices, and release instructions.

## Handling long runs

Before confirmation, print selected stages, upstream targets, expected downloads and
disk, SHA status, output/overwrite paths, cache reuse, and log directory.

During execution, stream stage progress, elapsed time, and best-effort ETA. On failure,
keep reusable caches and print the safe resume command.

## Pull-request checklist

- Scope matches the active phase.
- No consumer-owned transformations migrated.
- No heavy command launched without explicit permission.
- Scientific changes reflected in config and docs.
- Cheap tests pass.
- Report and figures exist.
- Generated data and secrets are absent.
- Licenses and attribution remain correct.
