# AGENTS.md

## Mission

Build a small, reproducible Windows 11 pipeline that regenerates the three external
PyPSA-Earth-derived NetCDF inputs consumed by `Fernando3161/EcuadorElectricGrid`:

1. `base.nc`
2. `solar.nc`
3. `onwind.nc`

Keep the boundary strict. `ec_network_2022.nc`, `network_base_filled.nc`, demand,
generation, hydro processing, expansion scenarios, and simulations remain in
`EcuadorElectricGrid`.

## Sources of truth

Read these before changing implementation or configuration:

1. `docs/PROJECT_OBJECTIVES.md`
2. `docs/CONSUMER_CONTRACT.md`
3. `docs/FORENSIC_BASELINE.md`
4. `docs/PROJECT_PLAN.md`
5. `config.yaml`
6. `config/legacy_pypsa_earth.yaml`

Treat the `scenarios` branch of `Fernando3161/EcuadorElectricGrid` and its Git history
as read-only evidence. Never modify that repository as part of this project.

## Non-negotiable execution safety

PyPSA-Earth, ERA5/atlite, OSM downloads, renewable-profile creation, and NetCDF
generation can consume hours, substantial bandwidth, memory, and disk.

- Never start a heavy target unless Fernando explicitly asks for that execution in
  the current conversation.
- Permission to implement, test, inspect, or continue a phase is not permission to
  run a heavy target.
- Do not run `all`, `base-network`, `renewables`, Snakemake targets that download or
  build data, ERA5/CDS retrieval, or a full integration test autonomously.
- Cheap actions are allowed: static checks, unit tests with tiny fixtures, config
  validation, imports, `--help`, dry-runs that provably do not download data, and
  inspection of already-existing metadata.
- Before a user-run heavy command, implement a preflight that validates disk space,
  credentials, configuration, target paths, upstream checkout, expected downloads,
  and overwrite decisions.
- At the end of each phase, state exactly which heavy commands were not run and give
  Fernando the terminal commands to run manually.

Do not hide subprocess output. Stream progress, stage names, log paths, elapsed time,
and best-effort ETA. On failure, retain logs and reusable downloads.

## Reproducibility rules

- Pin PyPSA-Earth to a full commit SHA after Phase 01 confirms it. The current SHA in
  `config.yaml` is only a forensic candidate.
- Record the effective configuration, upstream SHA, this repository SHA, environment
  export, source identifiers, timestamps, commands, file sizes, and SHA-256 checksums.
- Never use a moving branch, `latest`, or an unversioned URL as the only provenance.
- Cache downloads, but verify them before reuse.
- Never silently fall back to another source, weather year, spatial resolution, or
  network threshold.
- Make overwrites interactive by default. A non-interactive `--force` option may exist,
  but it must be explicit and shown in the provenance.
- Write outputs through temporary files and replace final files only after validation.
- A failed run must not leave a partial file under an authoritative output filename.
- Never commit generated `.nc` files, credentials, local environments, vendor clones,
  logs, or bulk downloads.

## Environment and interface

The primary platform is Windows 11 with PowerShell. The target public interface is
`run.ps1`, backed by a Python CLI. The bootstrap should:

1. create or reuse a repository-local environment at `.venv`;
2. acquire a pinned environment manager when no suitable local tool exists;
3. create the Conda-compatible geospatial environment from `environment.yml`;
4. clone or reuse PyPSA-Earth under `.vendor/pypsa-earth`;
5. check out the pinned commit and reject a dirty or unexpected upstream state;
6. run only the explicitly selected command.

Do not force this stack into `requirements.txt`. Use the Conda-compatible environment
as the authoritative dependency specification. Add a lock file for `win-64` once the
environment has been successfully solved and tested.

The CLI must eventually expose `check`, `base-network`, `renewables`, `all`, and
`validate`, plus `--help`. `check` must be cheap and must not trigger downloads.

## Development phases and branches

Work one phase at a time on the branch named in `docs/PROJECT_PLAN.md`. Do not jump
ahead merely because a later change is convenient.

Each phase must end with:

- its technical report in `reports/`;
- exact commands and software versions;
- decisions and deviations;
- validation evidence;
- multiple useful figures when the phase produces analysable evidence;
- a short manual-execution hand-off for Fernando.

Do not claim a phase complete while its acceptance criteria or report are missing.

## Testing policy

Keep automated testing minimal and high-value:

- configuration/schema validation;
- path and overwrite behavior;
- command dispatch and dry-run behavior;
- provenance/checksum creation;
- structural validation against tiny synthetic NetCDF fixtures.

CI must not download PyPSA-Earth data, ERA5, OSM extracts, or execute a real build.
Full generation is a manual integration test. Scientific validation is required for
released artifacts even though it is not part of CI.

## Coding practices

Prefer small Python modules and a thin CLI over notebooks for the production path.
Notebooks may be used only for exploration or report figures and must not be required
to reproduce artifacts.

Use `pathlib`, structured logging, explicit exceptions, and typed data structures.
Keep Windows paths safe; do not assume POSIX shells or sibling repository layouts.
Make stages restartable and idempotent. Separate orchestration, upstream execution,
artifact export, validation, and provenance.

Do not “fix” historical inconsistencies silently. Record them in the forensic report,
resolve them with evidence, and update both configuration and documentation.

## Scientific and legal integrity

Every figure and reported metric must be reproducible from a named artifact and code
revision. Label inferred or unverified statements as such.

Preserve upstream and source-data attribution. The MIT license covers original work
here; it does not relicense PyPSA-Earth or third-party data. If code is copied or
adapted from an incompatible license, keep its license notice and reassess repository
licensing before merging.
