# Contributing

Contributions are welcome when they preserve reproducibility, the consumer contract,
and the boundary between PyPSA-Earth data generation and Ecuador-specific modeling.

Read `AGENTS.md`, `docs/PROJECT_PLAN.md`, and `docs/DEVELOPMENT_WORKFLOW.md` first.
Use the active phase branch or a narrowly scoped feature branch. Keep CI cheap. Do not
commit generated NetCDF files, bulk downloads, credentials, environments, vendor
checkouts, or secret-bearing logs.

Scientific configuration, upstream, source, schema, or output-name changes require
matching provenance, validation, documentation, and report updates. Heavy workflows
must be run manually with commands, logs, checksums, and environment recorded.

Original contributions are distributed under MIT. Do not copy third-party code or data
without preserving notices and confirming license compatibility.
