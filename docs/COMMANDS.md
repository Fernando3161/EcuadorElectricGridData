# Commands

## Current state

There is no executable generator yet. These commands define the interface that
Phases 02–05 must implement; they are not evidence that a target already runs.

## Public PowerShell interface

```powershell
powershell -ExecutionPolicy Bypass -File .\run.ps1 check
powershell -ExecutionPolicy Bypass -File .\run.ps1 base-network
powershell -ExecutionPolicy Bypass -File .\run.ps1 renewables
powershell -ExecutionPolicy Bypass -File .\run.ps1 all
powershell -ExecutionPolicy Bypass -File .\run.ps1 validate
```

`check` is cheap and must not download bulk data. The three generation commands are
heavy and run only when explicitly selected. `validate` inspects existing artifacts.

## Required options

```text
--config PATH
--dry-run
--force
--non-interactive
--log-level LEVEL
--output-root PATH
```

`--dry-run` must guarantee no heavy subprocess or download. `--force` is recorded in
provenance. `--non-interactive` without `--force` fails if an output exists.

## Direct Python interface

After bootstrap:

```powershell
.\.venv\python.exe -m ecuador_grid_data --help
```

The PowerShell wrapper remains the supported clean-machine entry point.

## Manual-run hand-off

At the end of a phase, Codex gives Fernando the exact command, estimated download/disk
and time ranges, log location, expected success message and outputs, validation/resume
commands, and overwrite warning.

The wrapper should dispatch through the environment prefix; manual activation should
not be required.
