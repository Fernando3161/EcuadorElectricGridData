# Consumer Contract

## Consumer baseline

The compatibility target is:

- repository: `Fernando3161/EcuadorElectricGrid`
- branch: `scenarios`
- inspected head: `b57b5dc54cee1ad7a56f1c2da78db3cb6a3bce98`

The branch may later move. Changes to the consumer contract must cite the newly
inspected commit.

## Artifact ownership

| Artifact | Producer | Consumer path | Ownership |
|---|---|---|---|
| `base.nc` | PyPSA-Earth base-network rule | `data/raw/networks/base.nc` | This repository |
| `solar.nc` | PyPSA-Earth renewable-profile rule, exported under consumer name | `data/raw/cutouts/solar.nc` | This repository |
| `onwind.nc` | PyPSA-Earth renewable-profile rule, exported under consumer name | `data/raw/cutouts/onwind.nc` | This repository |
| `ec_network_2022.nc` | Ecuador network-cleaning notebook/code | `data/processed/networks/ec_network_2022.nc` | Consumer repository |
| `network_base_filled.nc` | Ecuador demand/generation attachment | `data/processed/networks/network_base_filled.nc` | Consumer repository |

The word `cutouts` in the consumer path is historical. `solar.nc` and `onwind.nc`
appear to be renewable-profile products, not raw atlite weather cutouts. Phase 01 must
prove the rename mapping before implementation.

## Directory hand-off

A release archive must contain:

```text
data/raw/networks/base.nc
data/raw/cutouts/solar.nc
data/raw/cutouts/onwind.nc
data/provenance/manifest.json
data/provenance/checksums.sha256
```

Extracting `data/` at the root of a compatible consumer checkout must require no path
edits.

## Base-network contract

At minimum:

- `pypsa.Network(path)` succeeds in the pinned compatibility environment;
- buses, lines, and transformers are present;
- bus coordinates are finite and geographically plausible for Ecuador;
- component indices and bus references survive NetCDF round-trip;
- nominal voltage data needed by consumer cleaning exist;
- the file is not already Ecuador-purged or enriched with consumer-side demand and
  generation.

Exact component counts, voltage classes, extents, and schema details are Phase 01/03
reference metrics, not values to guess in advance.

## Renewable-profile contract

The consumer opens each file with `xarray.open_dataset`, reads the `profile` data
variable, converts it to pandas, and assigns columns from the `bus` coordinate.

Therefore each file must have:

- a readable `profile` variable;
- a time dimension and a bus/region dimension compatible with `to_pandas()`;
- a `bus` coordinate containing the province/region identifiers used by the consumer;
- numeric values suitable for availability profiles;
- solar and wind column sets that can be aligned consistently;
- documented units and normalization.

The consumer code constructs an hourly index from
`2013-01-01 00:00` through `2013-12-31 23:00`, then relabels it to 2022. This strongly
suggests 8,760 non-leap-year samples. However, the archived override says
`2013-01-01` to `2013-01-15`. This is a release blocker until resolved with evidence.

## Compatibility test

Phase 05 must include a read-only test that:

1. places or maps the three artifacts at the exact paths;
2. loads `base.nc` with PyPSA;
3. executes only the consumer's artifact-loading and immediate schema assumptions;
4. does not run Ecuador scenario optimization or regenerate consumer-owned outputs;
5. records the consumer commit used.

Consumer-side transformations may reveal defects but must be fixed in their owning
repository unless the defect is an exported-data contract violation.
