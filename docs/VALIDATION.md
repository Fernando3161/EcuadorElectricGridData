# Validation

Automated tests remain minimal; released artifacts still require substantive
scientific and interface validation.

## Validation tiers

| Tier | Cost | Runs in CI | Purpose |
|---|---:|---:|---|
| Static/unit | seconds | Yes | Config, paths, dispatch, confirmation, provenance, checksums |
| Synthetic NetCDF | seconds | Yes | Validator behavior on tiny fixtures |
| Existing-artifact inspection | minutes | No, local | Structural checks without generation |
| Full upstream generation | hours | Never | Manual integration and scientific validation |
| Consumer compatibility | minutes to hours | Manual | Confirm exact downstream loading assumptions |

CI must not retrieve OSM, ERA5, cutout bundles, or other bulk data.

## Base-network validation

Required checks:

- file exists, is non-empty, and has SHA-256;
- `pypsa.Network` loads it without repair;
- buses, lines, and transformers exist;
- all line and transformer bus references resolve;
- coordinates are finite and within documented bounds;
- nominal voltages and required electrical fields are present;
- duplicated indices and isolated components are quantified;
- component counts and voltage classes are compared with any historical reference;
- no consumer-owned loads/generators were accidentally added.

Required figures include an Ecuador network map by voltage, component counts, voltage
distributions, connected-component sizes, coordinate diagnostics, and comparison with
a recovered reference when available.

## Renewable-profile validation

Required checks for both files:

- xarray opens the dataset;
- `profile` and `bus` exist;
- dimensions convert to the pandas form expected by the consumer;
- solar and wind use compatible bus sets;
- time is hourly, monotonic, unique, and covers the confirmed interval;
- no unexpected missing or infinite values;
- availability values satisfy documented bounds;
- province/region names match the consumer normalization;
- metadata records units, method, technology, and source weather.

Required figures include regional capacity factors, monthly/seasonal and diurnal
profiles, missingness heatmaps, value/clipping distributions, and a solar-versus-wind
regional comparison.

## Consumer compatibility validation

Use the pinned consumer commit. Load the files from exact destination paths and
exercise only the immediate import/schema logic. Confirm that no path edits are
needed, profile columns can be assigned from `bus`, and hourly indexing does not
silently become all-NaN.

## Validation output

Validators return nonzero on failure and write `validation.json` with validator
version, checksum, checks, metrics, severity, warnings, result, and figure paths.

Warnings affecting scientific meaning block release until resolved in the report.
