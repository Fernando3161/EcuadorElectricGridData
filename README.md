# EcuadorElectricGridData

Reproducible generation of the external PyPSA-Earth datasets required by
[EcuadorElectricGrid](https://github.com/Fernando3161/EcuadorElectricGrid).

This repository has a deliberately narrow boundary. It will generate and validate:

- `base.nc` — the PyPSA-Earth base transmission network for Ecuador.
- `solar.nc` — province-indexed solar availability profiles.
- `onwind.nc` — province-indexed onshore-wind availability profiles.

It will **not** generate `ec_network_2022.nc` or `network_base_filled.nc`. Those are
Ecuador-specific model products owned by the consumer repository.

## Status

The repository is currently in its documentation and forensic-reconstruction stage.
The historical evidence, target architecture, development phases, safety rules, and
consumer interface are specified, but the generator CLI has not yet been implemented.
Commands described as the “target interface” are therefore design requirements, not
commands that work in this initial commit.

The first development branch is `phase/01-forensics`. It must establish the exact
PyPSA-Earth commit, effective configuration, data-bundle path, target mapping, and
reference artifact characteristics without launching a heavy build.

## Intended interface

After implementation, a Windows 11 user should be able to run:

```powershell
powershell -ExecutionPolicy Bypass -File .\run.ps1 check
powershell -ExecutionPolicy Bypass -File .\run.ps1 base-network
powershell -ExecutionPolicy Bypass -File .\run.ps1 renewables
powershell -ExecutionPolicy Bypass -File .\run.ps1 all
powershell -ExecutionPolicy Bypass -File .\run.ps1 validate
```

`run.ps1` will create or reuse a repository-local environment, obtain a pinned
PyPSA-Earth checkout, validate prerequisites before expensive work, ask before
overwriting outputs, and print an unambiguous result.

## Output hand-off

The release/export bundle must preserve the consumer layout:

```text
data/
├── raw/
│   ├── networks/
│   │   └── base.nc
│   └── cutouts/
│       ├── solar.nc
│       └── onwind.nc
└── provenance/
    ├── manifest.json
    └── checksums.sha256
```

Copying or extracting that `data/` directory into `EcuadorElectricGrid` must satisfy
its external NetCDF requirements without changing consumer code.

## Documentation

- [Objectives](docs/PROJECT_OBJECTIVES.md)
- [Plan and phase branches](docs/PROJECT_PLAN.md)
- [Architecture](docs/ARCHITECTURE.md)
- [Consumer contract](docs/CONSUMER_CONTRACT.md)
- [Forensic baseline](docs/FORENSIC_BASELINE.md)
- [Configuration](docs/CONFIGURATION.md)
- [Reproducibility](docs/REPRODUCIBILITY.md)
- [Data sources and licensing](docs/DATA_SOURCES.md)
- [Validation](docs/VALIDATION.md)
- [Development workflow](docs/DEVELOPMENT_WORKFLOW.md)
- [Target commands](docs/COMMANDS.md)
- [Technical-report template](docs/TECHNICAL_REPORT_TEMPLATE.md)

## Platforms and licensing

Windows 11 (`win-64`, PowerShell) is the primary supported platform. Linux support
may be added where practical, but it must not be claimed until tested.

Original code and documentation in this repository are licensed under the MIT
License. PyPSA-Earth is a separate AGPL-3.0-or-later project. Source datasets and
generated artifacts retain their own attribution and licensing requirements; see
[DATA_SOURCES.md](docs/DATA_SOURCES.md).
