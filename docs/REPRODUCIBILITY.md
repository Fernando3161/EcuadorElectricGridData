# Reproducibility

## Reproducibility target

The goal is auditable regeneration, not a promise that mutable external services will
always return byte-identical data. The pipeline must pin every controllable input and
record every uncontrollable one.

## Levels

### Historical reconstruction

Identify what most likely produced the original artifacts. Historical uncertainty is
allowed only when labelled and justified.

### Computational reproduction

Given archived sources/cache, pinned code, locked environment, effective config, and
commands, produce the same artifact checksums.

### Scientific reproduction

If an upstream source can no longer provide identical bytes, generate a new,
versioned artifact that satisfies the same declared method and consumer contract,
then quantify differences.

Do not conflate these levels.

## Required provenance

For each generated file, store:

- logical artifact name and consumer path;
- SHA-256 and byte size;
- NetCDF format/engine and schema summary;
- creation start/end in UTC;
- exact command;
- project commit and dirty state;
- PyPSA-Earth commit and dirty state;
- environment lock and explicit package export;
- effective configuration file and SHA-256;
- input file/bundle identifiers, URLs, timestamps, sizes, and checksums;
- machine platform, Python version, thread count, and relevant resource settings;
- validation result and validator version;
- warnings, deviations, and use of `--force`.

The manifest must be machine-readable JSON. Reports may render the same facts for
humans but do not replace it.

## Bootstrap determinism

The final bootstrap should use a pinned, checksummed portable environment manager or
a verified preinstalled compatible manager. It creates `.venv` by prefix, not by a
global environment name, so repository state is self-contained.

PyPSA-Earth is cloned into `.vendor/pypsa-earth` and detached at a full SHA. The
bootstrap must refuse to proceed if the configured pin is absent, checkout SHA
differs, tracked upstream files are dirty, the environment lock does not match the
platform, or free disk is below the stage estimate.

## Data caching and restart

Downloads are stored separately from generated work. Each cache entry has source
metadata and a checksum. A reusable cache is accepted only after verification.

Stages write state records and may resume only from completed, validated stage
boundaries. A tool must not infer completion solely from filename presence.
Interrupted temporary files remain clearly non-authoritative.

## Release package

A Zenodo-ready release contains:

- the consumer-compatible `data/` tree;
- `manifest.json` and `checksums.sha256`;
- effective configuration;
- environment lock and explicit package list;
- technical reports and figures;
- README and citation metadata;
- source/third-party notices;
- exact project and upstream commit identifiers.

The archive itself also receives a SHA-256 recorded in the release notes.

## Reproduction log

Each manual heavy run should generate a timestamped run directory with
`command.txt`, `stdout.log`, `stderr.log`, `preflight.json`, `stage-state.json`,
`environment.txt`, `effective-config.yaml`, and `validation.json`.

Sensitive credentials and tokens must never appear in these files.
