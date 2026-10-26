# FormatFossil Architecture Research

## Pipeline
The planned experiment flow is: register source → identify format → capture tool environment → convert to a new output → validate → compare properties → report.

## Intended stack
- Python 3.13+ for CLI, orchestration, converter adapters, and comparison rules.
- Rust only for profiled performance-sensitive hashing or structural scans.
- JSON Schema and JSONL for experiment manifests and event/report records.
- SQLite for local experiment metadata.
- pytest, Cargo tests, golden fixtures, and GitHub Actions for verification.
- Docker for reproducible converter environments when it improves isolation and repeatability.
- JHOVE or other format-specific validators where supported; validators are adapters, not universal authorities.
- Optional Svelte + TypeScript viewer after result contracts stabilize.

## Experiment manifest
A manifest should record case ID, source path alias, source fingerprint, detected format and method, target format, tool name/version, command options, runtime image or OS, environment variables that affect output, start/end time, exit status, output fingerprint, validation results, comparison-rule version, and errors. Avoid storing secrets or full source content in the manifest.

## Comparison engine
Keep separate modules for byte comparison, metadata comparison, structural validation, semantic properties, and presentation metrics. Each module returns measured values, status (pass/fail/unknown/not applicable), supporting evidence, and tolerance/policy version. An unavailable comparator must return unknown rather than pass.

## Workspaces and trust
Use immutable source inputs and unique output directories. Treat converters and sample files as untrusted. Run tools in isolated child processes with timeouts, resource limits, and a minimal environment. Clean temporary files according to a documented policy.

## Extensibility
Each adapter should declare supported versions, input/output formats, measured properties, limitations, and fixture locations. Keep generic experiment orchestration independent of format-specific comparison logic. Add new format families only when a meaningful validation strategy exists.

## Performance
Hash streams rather than loading large files into memory. Bound archive expansion and file counts. Record truncated/incomplete experiments. Cache only by source hash, tool identity, options, and comparison version so stale outputs are not reused.

## Research decisions
Document exact equality rules, volatile-field exclusions, tolerance settings, supported validator coverage, and version-comparison policy. Every tolerance must have a rationale and tests at and beyond the boundary.
