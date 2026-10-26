# FormatFossil Validation Matrix

## Core cases

| Area | Test cases | Pass condition |
|---|---|---|
| Inventory | Extension disagrees with signature; unknown format | Detection method and uncertainty are explicit |
| Source integrity | Source file before/after conversion | Source hash unchanged |
| Converter execution | Success, failure, timeout, missing executable | Exit and failure status recorded distinctly |
| Output validation | Valid, malformed, truncated, unsupported format | Conformance status and validator coverage explicit |
| Byte comparison | Identical bytes, metadata-only byte change | Exact byte result is correct |
| Metadata | Known retained, modified, dropped fields | Each field has a stated outcome |
| Semantic properties | Dimensions, page count, stream count, timing, text content | Format-specific expected properties match |
| Round trip | A → B → A for selected fixtures | Losses and accumulated changes are reported |
| Tolerance | Values below, at, and above threshold | Consistent, documented decision |
| Security | Huge archive, malformed file, converter hang | Bounded execution and safe failure |
| Privacy | Sensitive metadata and excerpts | Default report omits unnecessary content |

## Golden fixtures
Build small fixtures per format with a known valid example, a malformed example, volatile metadata, meaningful metadata, a lossy-conversion case, a round-trip case, and a file where the extension lies. Use only synthetic or permitted samples.

## Acceptance criteria
- The source is never overwritten.
- Every output records tool/version and the comparison rules used.
- “Unknown” is not reported as “pass”.
- A successful converter exit does not imply fidelity.
- Expected losses are separated from unexpected changes.
- Reports identify unsupported properties and missing validators.
- Repeated execution with fixed tools and fixtures produces stable comparisons where determinism is expected.

## Metrics
Measure detection coverage, validator coverage, properties preserved/lost, report completeness, false-positive/negative counts against fixture labels, runtime, peak memory, and output reproducibility. State the format profile and tool version with every result.
