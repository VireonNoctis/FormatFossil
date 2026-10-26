# FormatFossil Mitigation Strategy

The objective is to detect and explain unintended conversion damage, not to promise that every file can be converted without loss.

| Risk | Control | Validation | Residual limitation |
|---|---|---|---|
| Exit code mistaken for success of fidelity | Compare multiple layers and validate output | Success-exit fixture deliberately drops a field | Some semantic properties cannot be automated |
| Format inferred from extension | Signature/parser-based identification where supported | Rename fixtures without changing contents | Ambiguous and polyglot files need special handling |
| Source overwritten | Immutable input path, separate output workspace, pre/post hash | Source hash remains stable | External converter may still affect OS state |
| Metadata silently lost | Per-format metadata inventory and field-diff report | Golden metadata fixtures | Some formats lack a complete metadata parser |
| Lossy behaviour labelled corruption | Define expected-loss profiles and tolerances | Test lossy and lossless cases separately | Expectations depend on user intent |
| False validator confidence | Publish module coverage and validation result | Unsupported-format tests report unknown | Validator defects remain possible |
| Converter version drift | Capture executable version, arguments, environment, and hash where feasible | Run same fixture across tool versions | External libraries may also change |
| Malicious document or converter | Isolated subprocess, timeout, memory/file limits, least privilege | Test timeout and malformed inputs | No sandbox is perfect |
| Sensitive document leak | Local-only processing by default; reports contain minimal excerpts | Scan default outputs for source content | Metadata can itself be sensitive |

## Operating safeguards
- Never alter the source file.
- Write converted outputs to a fresh, dedicated workspace.
- Validate output only after the converter exits and the output file is safely closed.
- Record every skipped comparison and why it was skipped.
- Keep measurement, policy judgement, and human review separate.
- Do not use one generic “fidelity score” to hide category-specific loss.
- Treat archives and documents as untrusted input.

## Release gates
A format adapter must define what it measures, what it ignores, expected losses, supported validator versions, failure semantics, and regression fixtures. A release can claim “no loss observed” only for explicitly named properties and test cases.
