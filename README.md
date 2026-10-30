# FormatFossil

<!-- REPOSITORY-HEALTH:START -->
[![Code quality](https://github.com/VireonNoctis/FormatFossil/actions/workflows/repository-health.yml/badge.svg?branch=main)](https://github.com/VireonNoctis/FormatFossil/actions/workflows/repository-health.yml)
[![Top language](https://img.shields.io/github/languages/top/VireonNoctis/FormatFossil?style=flat-square)](https://github.com/VireonNoctis/FormatFossil)
[![Languages](https://img.shields.io/github/languages/count/VireonNoctis/FormatFossil?style=flat-square)](https://github.com/VireonNoctis/FormatFossil)
[![Automated tech stack](https://img.shields.io/badge/tech_stack-auto--detected-64748b?style=flat-square)](TECH_STACK.md)
<!-- REPOSITORY-HEALTH:END -->


<p align="center">
  <strong>Conversion is a claim. Fidelity is something you test.</strong><br/>
  A file-conversion forensics laboratory for measuring what survives between formats, tools, and versions.
</p>

---

## Why FormatFossil exists

A conversion can succeed and still lose information. Images may change dimensions or colour characteristics; documents may lose layout, links, comments, or metadata; audio/video may change streams or timing; archives may alter paths, permissions, or timestamps. A successful exit code only says the program completed according to its own rules.

FormatFossil is planned to compare source and output files, validate their structure where tools exist, run round-trip experiments, and produce a field-by-field fidelity report. It aims to answer **what changed, what survived, what could not be checked, and which tool/version made the change**.

## Planned capabilities

- Reproducible conversion test cases with source hashes and explicit tool versions.
- Structural validation and format identification where reliable validators exist.
- Byte-level, metadata-level, and semantic comparisons kept separate.
- Round-trip workflows such as A → B → A with lossy and lossless behaviour labelled.
- Field-level loss reports for metadata, document structure, dimensions, streams, and content.
- Regression reports when converters or software versions change.
- Synthetic fixtures and permissioned sample files, rather than uncontrolled user documents.

## Workflow

1. Register a source artifact and compute its input fingerprint.
2. Identify the format from signatures and parsers, not filename extension alone.
3. Record the converter, arguments, environment, and version.
4. Convert into a fresh output path; never overwrite the source.
5. Run format validation where supported.
6. Compare bytes, structure, metadata, and semantic properties independently.
7. Optionally run a round trip and compare against the original baseline.
8. Export a report with losses, uncertainty, and evidence.

## Planned tech stack

| Area | Planned technology | Role |
|---|---|---|
| Experiment harness and CLI | Python 3.13+ | Run orchestration, adapters, comparison rules |
| Performance-critical file inspection | Rust, introduced only where benchmarks justify it | Bounded hashing and structural scanning |
| Results and contracts | JSONL, JSON Schema, SQLite | Reproducible records and local experiment history |
| Format validators | External validators such as JHOVE where applicable | Format-specific identification and validation |
| Tests | pytest, Cargo tests, golden fixtures | Fidelity and regression testing |
| Automation | GitHub Actions; Docker for reproducible tool environments | Repeatable CI jobs |
| Optional report viewer | Svelte + TypeScript | Review differences and conversion history |

Not every format has a trustworthy validator or a single definition of “same”. The stack and the fidelity rules will be selected per format family.

## Documentation

- [Research notes](docs/research-notes.md)
- [Mitigation strategy](docs/mitigation-strategy.md)
- [Architecture research](docs/architecture-research.md)
- [Validation matrix](docs/validation-matrix.md)

## Fidelity is not one number

| Comparison layer | Question |
|---|---|
| Byte identity | Are the output bytes identical to the expected bytes? |
| Structural validity | Does the output conform to the target format's rules? |
| Metadata preservation | Which fields survived, changed, or disappeared? |
| Semantic fidelity | Does the content convey or behave as intended? |
| Presentation fidelity | Did layout, colour, dimensions, timing, or rendering change? |
| Round-trip stability | What is lost after converting away and back? |

Byte inequality does not necessarily mean semantic damage: valid files can contain timestamps, reordered metadata, or different compression. Conversely, byte identity is not a substitute for checking whether the converter processed the intended input.

## Principles

- Never overwrite source artifacts.
- Hash inputs and outputs, and record exact tool versions and options.
- Separate measured loss from unsupported checks and subjective review.
- Keep expected lossy behaviour distinct from unintended corruption.
- Do not claim semantic equivalence without a format-specific test.
- Treat input documents and converter executables as untrusted; use isolated processes and bounded resources.
- Use synthetic or permissioned fixtures, with privacy-aware handling of user files.

## Status

**Research / early planning.** The documentation defines the intended methodology and tests. Feature claims should be updated only after implementation and repeatable evidence.

## Roadmap

- [ ] Define experiment manifest and result schemas.
- [ ] Implement a read-only inventory and stable hashing.
- [ ] Add a first format family with validator-backed checks.
- [ ] Implement byte, metadata, structural, and semantic comparison interfaces.
- [ ] Add round-trip experiment templates and golden fixtures.
- [ ] Add tool-version regression reports.
- [ ] Expand formats only when useful tests and known limitations are documented.

## Research references

- JHOVE documentation for format identification, validation, and characterization: https://jhove.openpreservation.org/documentation/
- UK National Archives, guidance on selecting file formats for long-term preservation: https://cdn.nationalarchives.gov.uk/documents/information-management/selecting-file-formats.pdf
- Unicode normalization FAQ, relevant to filename/text comparison: https://www.unicode.org/faq/normalization.html

## Contributing

A converter adapter needs a test fixture, tool/version capture, a stated fidelity model, and at least one negative test where conversion succeeds but an expected property is lost. Do not add copyrighted or private sample files without permission.

## License

Add an explicit licence before treating this repository as licensed for reuse.
