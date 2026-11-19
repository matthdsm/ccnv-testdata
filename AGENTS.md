# `copynumber-cnv-testdata` Agent Guidelines

This document defines repository maintenance rules for agents adding, modifying, or removing test datasets for `ccnv`.

## Mandatory Rules

1. **Keep the `README.md` Data Index Synchronized**:
   - Whenever you add, replace, rename, or delete any dataset, track, fixture, or truth manifest in this repository, you MUST update the `## Data index` section in `README.md` (including updating or removing fulfilled rows under `### Real-world data gaps (needed)`) in the same change.
   - Every index entry must include a repository-relative link to the file, its format/schema version (and index type where applicable), genome build or contig layout, key quantitative properties (e.g., bin size, pairing mode, sample count), and a concise description of what `ccnv` behavior or edge case it tests.

2. **No Paths or Clinical Sample Identifiers**:
   - Never persist internal filesystem paths, hostnames, or patient/clinical sample IDs in `.ccnv` Parquet metadata, samplesheets, or ground-truth manifests.
   - Validate all `.ccnv` artifacts against the active `ccnv` Parquet schema contract before finalizing changes.

3. **Keep Synthetic Fixtures Minimal and Deterministic**:
   - Prefer compact, seeded synthetic datasets over large real alignments whenever testing mechanical pipeline behavior (formats, indexes, contig naming, read filters, rebinning, edge cases).
   - Pair synthetic fixture suites with a machine-readable `truth.json` (or ground-truth BED/manifest) verified directly against `ccnv`.
