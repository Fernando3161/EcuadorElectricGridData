# Data Sources and Licensing

This inventory is provisional until Phase 01 reconstructs the effective upstream DAG.
The final manifest must list the exact sources actually reached by the pinned workflow.

## Expected sources

| Source | Role | Pinning requirement | Licensing/attribution action |
|---|---|---|---|
| OpenStreetMap / PyPSA-Earth-OSM | Transmission infrastructure feeding `base.nc` | Record extract URL/provider, timestamp or historical date, and checksum | Preserve OpenStreetMap attribution and review ODbL obligations |
| ERA5 via atlite or a PyPSA-Earth South America bundle | Weather input for solar and wind profiles | Record CDS/bundle record, cutout name, bounds, and checksum | Preserve Copernicus/ERA5 attribution and applicable terms |
| PyPSA-Earth data bundles | Boundaries, land cover, protected areas, and support data | Record bundle name, source URL/record, version/date, and checksum | Include every underlying source notice shipped with the bundle |
| Country boundaries used by the pinned workflow | Ecuador clipping and regionalization | Record dataset/version/layer and checksum | Verify redistribution terms before Zenodo publication |
| Technology definitions in atlite/PyPSA-Earth | PV and wind conversion | Pin code/config definitions | Cite atlite and PyPSA-Earth |

The historical upstream bundle configuration describes
`bundle_cutouts_southamerica`, producing `cutouts/cutout-2013-era5.nc` from a Google
Drive source and notes an approximate 18 GB compressed size. Phase 01 must verify that
the link, bytes, and legal notices still correspond to the historical workflow.

## Source policy

- Prefer stable records with versioned URLs or DOIs.
- Record a checksum even when a source has a DOI.
- If a source is mutable, record retrieval time and retain the permitted raw input or
  document why it cannot be redistributed.
- Never automate acceptance of third-party terms or credential creation.
- Never commit credentials, CDS API tokens, or downloaded bulk data.
- Do not publish a Zenodo archive until redistribution rights for every included
  artifact and bundled notice have been reviewed.

## License boundary

The MIT license applies to original orchestration code and documentation. It does not
relicense PyPSA-Earth, OpenStreetMap-derived data, ERA5/Copernicus data, GADM or other
boundary datasets, or other upstream files.

If implementation copies or adapts PyPSA-Earth code rather than invoking it, preserve
upstream SPDX headers and evaluate AGPL obligations before merging. Invoking a pinned
upstream checkout is preferred.

## Publication checklist

Before Zenodo upload:

1. enumerate deposit files;
2. map each to its immediate and upstream sources;
3. include required attribution and notices;
4. verify redistribution rights;
5. distinguish software from dataset licenses;
6. cite PyPSA-Earth, PyPSA, atlite, OpenStreetMap, ERA5, and effective sources;
7. record the Zenodo DOI in `CITATION.cff`.
