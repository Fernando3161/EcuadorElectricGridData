# Forensic Baseline

This document records what is known before implementation. It distinguishes direct
evidence from inference so later Codex sessions do not turn a plausible guess into a
pin.

## Direct evidence

### Consumer repository

The inspected `scenarios` head is
`b57b5dc54cee1ad7a56f1c2da78db3cb6a3bce98` (12 May 2026).

Its path helper defines `data/raw/networks`, `data/raw/cutouts`, and
`data/processed/networks`. Its `.gitignore` excludes `*.nc`, explaining why the
NetCDF artifacts are absent.

### First committed successful `base.nc` load

Commit `156111c5aae86509bfa2ff741943bde079963b9b` dated 5 November 2025 added
`notebooks/05_EC_network_eval.ipynb`. The saved output states that PyPSA imported
`base.nc` with buses, lines, and transformers and loaded:

```text
C:\Repositories\Repos\pypsa-earth-project\EcuadorElectricGrid\data\raw\networks\base.nc
```

Notebook metadata reports Python `3.11.14`. Later output paths show the Conda
environment name `pypsa-earth-ec`.

This is the current earliest proof in Git that `base.nc` existed and loaded
successfully. The file itself was never committed.

### Historical configuration and environment

Commit `86a5025ecb005d326b274902f9a43c05ccf7e875` dated 4 November 2025 added
`config/config.yaml` and `config/environment_ec.yaml`. The relevant configuration has
remained unchanged on the inspected branch.

The environment closely matches PyPSA-Earth's upstream environment at
`5a44728fb82a4e41adcd977765fb1df7a5bc4e5e`, except for:

- environment name `pypsa-earth-ec`;
- added `geoplot`;
- an apparent typo `ploty=6.3.1`;
- one empty dependency item.

The reconstructed `environment.yml` fixes only the obvious typo and empty item and
remains provisional until it is solved and locked.

### Upstream candidate

At the time of the first successful consumer proof, the latest PyPSA-Earth commit was
`5a44728fb82a4e41adcd977765fb1df7a5bc4e5e` (30 October 2025). At that commit:

- `config.default.yaml` declares version `0.7.0`;
- the environment constraints match the archived environment as described above;
- the base-network output is `networks/base.nc`;
- renewable-profile outputs are
  `resources/renewable_profiles/profile_{technology}.nc`;
- `bundle_cutouts_southamerica` provides `cutouts/cutout-2013-era5.nc` and is described
  as approximately 18 GB.

This SHA is a strong candidate and an upper-bound reconstruction, but it is **not yet
proven to be the exact checkout used**. `config.yaml` therefore keeps
`pinned_commit: null`.

### Consumer-side ownership

The current network-cleaning notebook reads `data/raw/networks/base.nc` and exports
`data/processed/networks/ec_network_2022.nc`.

The later network-building notebook reads `ec_network_2022.nc`, consumer-owned demand
and generation data, and `data/raw/cutouts/solar.nc` plus `onwind.nc`; it exports
`network_base_filled.nc`.

Therefore only `base.nc`, `solar.nc`, and `onwind.nc` belong here.

## Working hypotheses to test

1. The historical PyPSA-Earth checkout was at, or functionally close to,
   `5a44728fb82a4e41adcd977765fb1df7a5bc4e5e`.
2. `solar.nc` is an unchanged rename/copy of
   `resources/renewable_profiles/profile_solar.nc`.
3. `onwind.nc` is an unchanged rename/copy of
   `resources/renewable_profiles/profile_onwind.nc`.
4. The South America prebuilt atlite cutout supplied the weather input.
5. The consumer profiles contain a complete 2013 hourly year.

None becomes an implementation assumption until Phase 01 records supporting evidence.

## Known inconsistencies and risks

- The historical override defines an atlite cutout named
  `cutout-2013-era5-tutorial`, while upstream renewable defaults point to
  `cutout-2013-era5`.
- The override contains a comment that the South America cutout bundle is needed, but
  all retrieval/download flags are false.
- The archived snapshots span only 1–15 January 2013, while consumer code appears to
  expect the complete year.
- `atlite` and `snapshots` appear nested under `electricity` in the archived override,
  while the candidate upstream defaults define them at top level.
- The historical configuration is an override; many effective values came from
  `config.default.yaml`.
- The original local PyPSA-Earth Git metadata and NetCDF checksums are unavailable on
  the present laptop.
- Upstream source datasets may have changed even when workflow code is pinned.

## Phase 01 evidence strategy

Use, in order:

1. consumer Git history and saved notebook outputs;
2. archived environment/config signatures matched across upstream commits;
3. upstream rule paths, bundle definitions, and configuration version;
4. filesystem timestamps or old logs if Fernando later finds another disk/backup;
5. structural metadata from any recovered NetCDF or Zenodo candidate;
6. a cheap Snakemake DAG/dry-run only after proving it performs no retrieval.

If exact historical identity cannot be established, choose and document a
**reproduction baseline** distinct from the **historical candidate**. Never call the
two equivalent.
