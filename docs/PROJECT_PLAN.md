# Project Plan

Development is phase-gated. Each phase uses its own branch, ends with a technical
report, and merges only after its acceptance criteria are met. Heavy execution is
manual and requires Fernando's explicit request.

## Phase 01 — Forensic reconstruction

**Branch:** `phase/01-forensics`  
**Heavy execution:** forbidden.

Tasks:

- Trace the earliest successful use of each target artifact in the full consumer Git
  history.
- Determine the most likely PyPSA-Earth checkout and verify it using file signatures,
  configuration version, rules, filenames, and environment constraints.
- Reconstruct the effective configuration after upstream defaults and local overrides.
- Establish how `bundle_cutouts_southamerica` was obtained.
- Confirm whether `solar.nc` and `onwind.nc` are renamed copies of
  `profile_solar.nc` and `profile_onwind.nc`.
- Resolve the historical 15-day snapshot configuration versus the consumer's apparent
  8,760-hour expectation.
- Define reference structural metrics obtainable without regenerating data.
- Record unresolved facts explicitly.

Deliverables:

- `reports/phase-01-forensics.md`
- configuration-diff and provenance-timeline figures
- confirmed upstream SHA or a documented escalation if proof remains insufficient
- updated `config.yaml` and effective PyPSA-Earth override

Acceptance criteria:

- no important inference is presented as fact;
- every pinned value has evidence;
- all blockers for implementation are enumerated;
- no heavy workflow was run.

## Phase 02 — Bootstrap and CLI skeleton

**Branch:** `phase/02-bootstrap`  
**Heavy execution:** forbidden.

Tasks:

- Implement `run.ps1` and the Python CLI.
- Bootstrap a local `.venv` Conda-compatible prefix and pinned tool acquisition.
- Clone/check out PyPSA-Earth under `.vendor/pypsa-earth`.
- Implement `check`, configuration validation, dry-run, logging, confirmation,
  stage state, and provenance scaffolding.
- Solve and lock the environment on `win-64`.
- Add minimal fixture-based tests and cheap CI.

Deliverables:

- `reports/phase-02-bootstrap.md`
- bootstrap timing and dependency/environment figures
- exact manual commands for Fernando

Acceptance criteria:

- a clean-machine bootstrap check succeeds on Windows 11;
- `check` performs no heavy download;
- heavy commands require explicit selection;
- CI remains cheap.

## Phase 03 — Base network

**Branch:** `phase/03-base-network`  
**Heavy execution:** user-run only.

Tasks:

- Map the CLI stage to the exact pinned PyPSA-Earth `base.nc` target.
- Implement prerequisite discovery and verified caching.
- Export atomically to `data/raw/networks/base.nc`.
- Implement structural validation and provenance.
- Guide Fernando through the first real run and diagnose returned logs.

Deliverables:

- `reports/phase-03-base-network.md`
- network map, voltage/component distributions, topology diagnostics, and comparison
  figures
- checksummed validated `base.nc` outside Git

Acceptance criteria:

- the consumer can load the artifact with PyPSA;
- expected components and coordinates are present;
- no partial output can masquerade as success;
- configuration and source state are fully recorded.

## Phase 04 — Renewable profiles

**Branch:** `phase/04-renewable-profiles`  
**Heavy execution:** user-run only.

Tasks:

- Obtain or build the exact weather cutout through the confirmed historical path.
- Generate solar and onshore-wind profiles with pinned rules and configuration.
- Map upstream profile filenames to the consumer filenames without altering contents.
- Validate dimensions, coordinates, time index, bounds, missing values, and geographic
  coverage.
- Support independent resume of solar and wind stages.

Deliverables:

- `reports/phase-04-renewable-profiles.md`
- spatial capacity-factor maps, seasonal profiles, diurnal profiles, missing-data
  diagnostics, and source/configuration figures
- checksummed `solar.nc` and `onwind.nc` outside Git

Acceptance criteria:

- both datasets satisfy the consumer contract;
- the 2013/full-year issue is conclusively resolved;
- source licensing and weather-data provenance are recorded.

## Phase 05 — Integration, release, and Zenodo package

**Branch:** `phase/05-validation-release`  
**Heavy execution:** user-run only.

Tasks:

- Execute the consumer-side compatibility check without migrating consumer logic.
- Build the final `data/` tree, manifest, checksums, environment export, notices, and
  reports.
- Test recovery, overwrite confirmation, cached rerun, and clean-machine instructions.
- Prepare—but do not automatically upload—the Zenodo deposit package.
- Document the Zenodo DOI/record fields and update citation metadata after publication.

Deliverables:

- `reports/phase-05-validation-release.md`
- end-to-end validation and reproducibility figures
- versioned Zenodo-ready archive
- release notes and final commands

Acceptance criteria:

- all three artifacts validate and land at exact consumer paths;
- a second person can follow the instructions;
- the package contains enough provenance to regenerate or audit every artifact;
- publication remains a deliberate manual action.

## Phase report rule

Every report follows `docs/TECHNICAL_REPORT_TEMPLATE.md`. Figures must answer real
technical questions, not decorate the document. Record commands, timings, machine
context, input identifiers, checksums, deviations, and failures as well as successes.
