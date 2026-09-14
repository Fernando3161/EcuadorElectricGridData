# Technical Report Template

Copy this file to `reports/phase-XX-name.md`.

## Metadata

- Phase and branch:
- Report date:
- Author/reviewer:
- Project commit:
- PyPSA-Earth commit:
- Consumer commit:
- Platform:
- Environment lock/checksum:
- Effective-config checksum:

## Executive result

State what was established or produced, what remains unresolved, and whether the phase
acceptance criteria passed.

## Scope

List included and excluded work. State the heavy-execution permission and which heavy
commands, if any, were run manually by Fernando.

## Evidence and method

Describe inputs, source identifiers, Git-history evidence, commands, assumptions, and
methods. Separate observed facts from inferences.

## Configuration and deviations

Provide a concise configuration table. Explain every deviation from historical
evidence, the plan, or upstream defaults.

## Results

Report quantitative metrics and link each to machine-readable validation or logs.

## Figures

Include multiple figures answering the phase's technical questions. For every figure
give the question answered, input artifact/checksum, generating code/commit,
transformations, interpretation, and limitations.

## Verification

Record cheap tests, manual commands and exit codes, artifact paths/sizes/checksums,
validation summary, and consumer compatibility where applicable.

## Performance and recoverability

Record stage duration, available CPU/RAM/disk context, downloads, cache hits, restart
behavior, and interruptions.

## Problems and resolutions

Include unsuccessful attempts affecting reproducibility. State what cached or partial
products remain and how to handle them safely.

## Acceptance criteria

Copy the phase criteria and mark each pass/fail with evidence.

## Manual commands for Fernando

Give exact PowerShell commands, outputs, logs, and overwrite behavior.

## Open issues and next phase

List unresolved risks, decisions required from Fernando, and the permitted next scope.
