# FormatFossil Research Notes

## Research question
How can a conversion pipeline measure what survived file transformation, without reducing fidelity to a single checksum or a converter's success exit code?

## Key distinction: identification, validation, characterization
JHOVE documents three useful operations: identifying a format, validating conformance, and characterizing properties. Conformance can include syntactic well-formedness and deeper validity. A filename extension is only a hint; signatures and format parsers provide better evidence.
Reference: https://jhove.openpreservation.org/documentation/

**Design implication:** identify from file content where possible and report validator coverage. “Validator unavailable” must not be presented as “valid”.

## Preservation requires context
The UK National Archives' guidance discusses format properties relevant to long-term preservation, including disclosure, stability, validation tooling, documentation, and metadata support.
Reference: https://cdn.nationalarchives.gov.uk/documents/information-management/selecting-file-formats.pdf

**Design implication:** capture tool name/version, options, target format profile, source/output fingerprints, and relevant environment. A change may be acceptable, expected, or destructive depending on the property being tested.

## Text and filenames have equivalence traps
Unicode supports canonical-equivalent strings with different byte representations. Filesystem and conversion tools may normalize or preserve names differently.
Reference: https://www.unicode.org/faq/normalization.html

**Design implication:** compare raw bytes and normalized textual comparisons separately. Never normalize and overwrite the only copy of a source name or text field.

## Fidelity dimensions

- Byte identity: exact bytes.
- Structural validity: conformance to format rules.
- Metadata preservation: fields retained, changed, or dropped.
- Semantic fidelity: the meaning/content needed by a task remains.
- Presentation fidelity: layout, colour, resolution, timing, or rendering remains within a stated tolerance.
- Round-trip behaviour: what changes after A → B → A.

A conversion can be byte-different yet semantically equivalent; it can also return exit code zero while dropping metadata or altering structure.

## Threats to validity

| Threat | Mitigation in research design |
|---|---|
| Unstable metadata timestamps | Separate volatile fields from fidelity-relevant fields and document exclusions |
| Lossy conversion expected by design | Define tolerances and expected-loss profile before running |
| Validator supports only part of a format | Report module and coverage; do not infer full validity |
| Converter version changes defaults | Capture exact version and arguments |
| Reference comparison is subjective | Use explicit metrics and reviewed golden fixtures |
| Input file is malformed | Record failure separately from conversion loss |
| Format parser is vulnerable | Run isolated with resource limits and read-only input |

## Open questions
1. Which format family offers the best initial scope for rigorous testing?
2. Which metadata fields matter for each format and use case?
3. How should visual and audio tolerances be declared and compared?
4. How can validators be invoked reproducibly across operating systems?
5. What is the minimum evidence required to call a conversion “lossless”?

**Research rule:** “lossless” must always name the properties and test suite under which loss was not observed.
