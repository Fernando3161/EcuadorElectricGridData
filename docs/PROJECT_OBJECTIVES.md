# Project Objectives

## Purpose

`EcuadorElectricGridData` exists to make three otherwise-missing external inputs of
`EcuadorElectricGrid` independently reproducible. A new user should not need the
original author's old laptop, undocumented manual copying, or a sibling checkout of
the consumer repository.

## Primary objective

Provide a one-command, restartable Windows 11 workflow that recreates:

| Artifact | Meaning | Consumer destination |
|---|---|---|
| `base.nc` | PyPSA-Earth Ecuador base grid from OSM-derived infrastructure | `data/raw/networks/base.nc` |
| `solar.nc` | Solar availability profiles indexed compatibly with the consumer | `data/raw/cutouts/solar.nc` |
| `onwind.nc` | Onshore-wind availability profiles indexed compatibly with the consumer | `data/raw/cutouts/onwind.nc` |

The generated release bundle must be suitable for manual publication on Zenodo with
persistent identifiers, checksums, configuration, provenance, and validation evidence.

## Secondary objectives

- Reconstruct and pin the historical PyPSA-Earth revision and effective configuration.
- Bootstrap a repository-local environment and upstream checkout from one PowerShell
  command.
- Separate cheap preflight checks from explicitly user-launched heavy work.
- Cache verified downloads and support safe restart after interruption.
- Ask before overwriting an authoritative artifact.
- Produce technical reports with meaningful figures after every development phase.
- Keep CI fast and inexpensive while retaining strong scientific validation for
  manually generated releases.
- Make future reruns understandable by someone who did not participate in the original
  work.

## Non-objectives

This repository does not:

- clean or adapt `base.nc` into `ec_network_2022.nc`;
- attach Ecuadorian power plants, demand, or hydro profiles;
- produce `network_base_filled.nc`;
- implement grid-expansion scenarios or optimization;
- replace PyPSA-Earth;
- publish automatically to Zenodo in the initial scope;
- promise bit-for-bit identity across unpinned source-data updates.

Those functions remain in `EcuadorElectricGrid` or in upstream systems.

## Success criteria

The project is successful when a clean Windows 11 machine can:

1. clone this repository;
2. run one documented PowerShell command;
3. receive an early, actionable failure if prerequisites or credentials are missing;
4. generate the three artifacts without editing source code;
5. rerun without redownloading verified inputs;
6. choose whether to overwrite existing outputs;
7. validate structural and scientific expectations;
8. create a release directory with a manifest, checksums, reports, figures, licenses,
   and exact provenance;
9. place the exported `data/` tree in `EcuadorElectricGrid` without consumer code
   changes.

## Quality priorities

In order: provenance, interface compatibility, correctness, recoverability, clarity,
and then speed. A clear failure is better than silently producing a plausible but
incompatible dataset.
