# Configuration

## Files

`config.yaml` controls this repository: platform, consumer contract, upstream pin,
environment, stage behavior, output paths, and reporting.

`config/legacy_pypsa_earth.yaml` preserves the override found in the consumer
repository. It is evidence, not yet the final runnable override.

`environment.yml` reconstructs the historical Conda-compatible environment and fixes
two obvious transcription errors. Phase 02 must produce a tested `win-64` lock file.

A future `config/pypsa_earth.yaml` will contain the confirmed effective override used
by the generator. It must be created from forensic findings, not by silently editing
the legacy evidence file.

## Configuration precedence

The planned precedence is:

1. immutable PyPSA-Earth defaults at the pinned commit;
2. generated effective project override;
3. explicit CLI flags limited to operational choices such as target, paths, `--force`,
   and logging;
4. no environment-variable override for scientific parameters.

Every effective configuration is copied into the artifact manifest and hashed.

## Important inherited values

The archived override supplies:

- country `EC`;
- tutorial mode off;
- OSM/network threshold of 35 kV;
- CRS values EPSG:4326, EPSG:3857, and ESRI:54009;
- ERA5 weather year 2013;
- 0.3° atlite grid spacing;
- four atlite processes;
- renewable carriers including solar and onshore wind;
- specific AC/DC line types and length factor 1.25;
- under-construction treatment `zero`.

Additional solar/onshore-wind resource settings were inherited from upstream
`config.default.yaml` version 0.7.0. Phase 01 must snapshot the complete effective
values, including turbine, panel, orientation, land-use filters, correction factors,
capacity density, clipping, cutout name, and snapshot inclusivity.

## Parameters that must not remain ambiguous

Before any heavy run, all of the following must be explicit:

- full upstream commit SHA;
- upstream config version;
- exact snapshot interval and inclusive-end behavior;
- raw-cutout source and checksum;
- OSM data source/date or extract identifier;
- country boundary source/version;
- voltage thresholds;
- solar panel and orientation;
- wind turbine;
- land-cover/protected-area exclusions;
- upstream targets and export-name mapping;
- environment lock hash;
- download/cache and output directories.

## Safe changes

A scientific configuration change creates a new artifact version. It must update the
configuration, relevant report, manifest, validation expectations, and release notes.

Never overwrite a published version's provenance. A new weather year, resolution,
network threshold, source-data snapshot, or upstream commit is a new dataset release,
not a transparent maintenance edit.
