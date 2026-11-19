# copynumber-cnv-testdata

Test datasets and fixtures for [`ccnv`](https://github.com/matthdsm/copynumber-cnv).

## Data index

### Extract (`extract/sample_data/`)

Minimal deterministic synthetic alignment fixtures (`136 KB` total) for end-to-end testing of `ccnv extract` across file formats, index types, contig naming conventions, read pairing modes, and filter flags.

| File | Format / Index | Layout / Build | Description |
| --- | --- | --- | --- |
| [`extract/sample_data/synthetic_ref.fa`](extract/sample_data/synthetic_ref.fa) | FASTA (`.fai`) | Synthetic UCSC (`chr1` 25 kb, `chr2` 12,345 bp, `chrX` 10 kb, `chrY` 10 kb, `chrM` 5 kb, `chrUn_decoy` 5 kb) | Minimal 67.3 kb reference genome with varying per-bin GC content, a partial trailing bin on `chr2`, sex contigs, and non-canonical contigs. |
| [`extract/sample_data/paired_end.bam`](extract/sample_data/paired_end.bam) | BAM (`.bai`) | Synthetic UCSC (6 contigs) | Paired-end proper-pair reads (`80` pairs total; `72` canonical, `8` non-canonical) spanning short (`[80, 150]` bp), mononucleosomal, dinucleosomal, and high-MW fragment bands; tests BAM + `.bai` extraction, multi-threaded tiling (`-t > 1`), fragmentomics, and `--all-contigs`. |
| [`extract/sample_data/paired_end.cram`](extract/sample_data/paired_end.cram) | CRAM (`.crai`) | Synthetic UCSC (6 contigs, ref `synthetic_ref.fa`) | Bit-for-bit identical reads to `paired_end.bam`; tests CRAM + `--fasta`, `.crai` tile streaming, and BAM/CRAM extraction parity. |
| [`extract/sample_data/paired_end_csi.bam`](extract/sample_data/paired_end_csi.bam) | BAM (`.csi` only) | Synthetic UCSC (6 contigs) | Identical reads to `paired_end.bam` indexed strictly with `.csi` (no `.bai` sibling) to exercise the CSI index reader path. |
| [`extract/sample_data/paired_end_unindexed.bam`](extract/sample_data/paired_end_unindexed.bam) | BAM (unindexed) | Synthetic UCSC (6 contigs) | Identical reads to `paired_end.bam` with no index; tests index-free single-threaded streaming (`-t 1`) and missing-index rejection (`-t > 1`). |
| [`extract/sample_data/single_end.bam`](extract/sample_data/single_end.bam) | BAM (`.bai`) | Synthetic UCSC (6 contigs) | Single-end reads (`53` canonical, `6` non-canonical) testing 0-based `reference_start` binning, `paired_end=false` metadata, null `frag_bands`, and mixed PE/SE cohort rejection. |
| [`extract/sample_data/plain_contigs_filters.bam`](extract/sample_data/plain_contigs_filters.bam) | BAM (`.bai`) | Synthetic Ensembl (`1`, `2`, `X`, `Y`, `MT`, `GL000191.1`) | Plain contig names plus deterministic filter torture reads (duplicates, MAPQ 0 and MAPQ 5, secondary, supplementary, improper pairs, placed and unplaced unmapped reads) to verify all `ReadFilters` flags and `ccnv.qc` counters. |
| [`extract/sample_data/truth.json`](extract/sample_data/truth.json) | JSON | — | Verified expected row counts, bin count vectors, contig orders/lengths, `paired_end` flags, and `ccnv.qc` counters across default, `--all-contigs`, relaxed-filter, and `--min-mapq 10` runs. |

### Pre-extracted samples (`extract/schema_2.0/`)

| File | Schema | Build / Layout | Key Properties | Description |
| --- | --- | --- | --- | --- |
| [`extract/schema_2.0/sample3_equCab-T2T.ccnv`](extract/schema_2.0/sample3_equCab-T2T.ccnv) | `2.0.0` (sample) | `equCab-T2T` (32 contigs: `chr1`–`chr31`, `chrX`) | 5 kb bins, paired-end, `14.66M` kept reads | Non-human equine WGS sample sketch for organism-agnostic rebinning and prediction tests against `flat_equCab-T2T.100kb.ccnv`. |
| [`extract/schema_2.0/sample4_GRCh38-noalt-WGS.ccnv`](extract/schema_2.0/sample4_GRCh38-noalt-WGS.ccnv) | `2.0.0` (sample) | `GRCh38-noalt` (24 contigs: `chr1`–`chr22`, `chrX`, `chrY`) | 5 kb bins, paired-end, `457.0M` kept reads | Deep human WGS sample sketch for high-coverage rebinning and germline prediction tests against `WGS-full-pass.GRCh38-noalt.10kb.ccnv`. |

### Covariate and plot tracks (`tracks/`)

Minimal synthetic tracks (`28 KB` total) aligned to the 6-contig synthetic cohort genome (`chr1`–`chr4` 625 kb each, `chrX` 1.8 Mb, `chrY` 300 kb).

| File | Format | Layout | Description |
| --- | --- | --- | --- |
| [`tracks/synthetic_cohort_gc.bw`](tracks/synthetic_cohort_gc.bw) | BigWig | Synthetic 6-contig (5 kb bins) | Smooth GC content track (`0.40`–`0.54`) testing `bigtools` BigWig GC ingestion and Poisson covariate normalization. |
| [`tracks/synthetic_cohort_gc.bed.gz`](tracks/synthetic_cohort_gc.bed.gz) | Gzipped BED/bedGraph | Synthetic 6-contig (5 kb bins) | Identical GC values to `synthetic_cohort_gc.bw` for verifying BED/BigWig GC loader parity. |
| [`tracks/synthetic_cohort_mappability.bw`](tracks/synthetic_cohort_mappability.bw) | BigWig | Synthetic 6-contig (5 kb bins) | Mappability track (`0.95` baseline with deliberate `0.15`–`0.20` low-mappability bins on every contig) testing `--mappability` and `--min-mappability` masking. |
| [`tracks/synthetic_cohort_cytobands.bed.gz`](tracks/synthetic_cohort_cytobands.bed.gz) | Gzipped UCSC cytoband BED | Synthetic 6-contig (`p12`/`p11`/`q11`/`q12`) | Synthetic cytoband ideogram track testing `--cytobands` override in interactive HTML plot output. |

### Synthetic reference cohort and `ref-qc` suite (`reference/synthetic_cohort/`)

Minimal deterministic cohort (`640 KB` total) for end-to-end testing of `ccnv reference`, `ccnv ref-qc`, samplesheet formats, rebinning, and schema rejection.

| File | Format / Schema | Layout / Mode | Key Properties | Description |
| --- | --- | --- | --- | --- |
| [`reference/synthetic_cohort/samples/`](reference/synthetic_cohort/samples/) | `2.0.0` (12 samples) | Synthetic 6-contig | 5 kb bins, paired-end, 6 XX (`ctrl_f01`–`f06`) + 6 XY (`ctrl_m01`–`m06`) | Minimal balanced control cohort for `ccnv reference` end-to-end tests, 2D sex inference, and covariate normalization. |
| [`reference/synthetic_cohort/samplesheet_germline.yaml`](reference/synthetic_cohort/samplesheet_germline.yaml) | YAML | 12 entries (unlabeled `sex`) | Relative paths to `samples/` | Tests YAML samplesheet loading and automated 2D sex inference. |
| [`reference/synthetic_cohort/samplesheet_germline.json`](reference/synthetic_cohort/samplesheet_germline.json) | JSON | 12 entries (explicit `sex`) | Relative paths to `samples/` | Tests JSON samplesheet loading and explicit `sex` branch assignment. |
| [`reference/synthetic_cohort/single_end_ctrl.ccnv`](reference/synthetic_cohort/single_end_ctrl.ccnv) | `2.0.0` (sample) | Synthetic 6-contig | 5 kb bins, single-end (`paired_end=false`) | Matching single-end sample for verifying mixed paired-end/single-end cohort rejection. |
| [`reference/synthetic_cohort/non_nested_7kb_sample.ccnv`](reference/synthetic_cohort/non_nested_7kb_sample.ccnv) | `2.0.0` (sample) | Synthetic 6-contig | 7 kb bins, paired-end | Non-nested bin grid sample testing cumulative knot interpolation and fragmentomics stripping during rebinning. |
| [`reference/synthetic_cohort/germline_ref_5kb.ccnv`](reference/synthetic_cohort/germline_ref_5kb.ccnv) | `2.0.0` (reference) | Synthetic 6-contig / `germline` | 5 kb bins, `12` samples, GC + mappability + fragmentomics corrected | Full synthetic germline reference (`autosomal`, `female_sex`, `male_sex`) backing `predict/germline/synthetic/`. |
| [`reference/synthetic_cohort/ref_qc_pass_autosomal.ccnv`](reference/synthetic_cohort/ref_qc_pass_autosomal.ccnv) | `2.0.0` (reference) | Synthetic 8-autosome / `germline` | 5 kb bins, `16` samples (`autosomal` branch, SE) | Naturally healthy reference fixture generated directly by `ccnv reference` to produce `ref-qc` verdict `PASS`. |
| [`reference/synthetic_cohort/ref_qc_warn_flat.ccnv`](reference/synthetic_cohort/ref_qc_warn_flat.ccnv) | `2.0.0` (reference) | Synthetic UCSC (`synthetic_ref.fa`) / flat | 5 kb bins, `0` samples (`flat=true`) | Flat FASTA reference fixture verified to produce `ref-qc` verdict `WARN`. |
| [`reference/synthetic_cohort/ref_qc_fail_masked_male.ccnv`](reference/synthetic_cohort/ref_qc_fail_masked_male.ccnv) | `2.0.0` (reference) | Synthetic 6-contig / `germline` | 5 kb bins, `100%` masked `chrY` in `male_sex` | Degraded reference fixture verified to produce `ref-qc` verdict `FAIL`. |
| [`reference/synthetic_cohort/incompatible_v1_sample.ccnv`](reference/synthetic_cohort/incompatible_v1_sample.ccnv) | `1.0.0` (legacy sample) | `chr1` (10 kb) | 5 kb bins, incompatible schema | Legacy v1 Parquet fixture verifying strict `SchemaVersionError` rejection. |
| [`reference/synthetic_cohort/truth.json`](reference/synthetic_cohort/truth.json) | JSON | — | — | Verified `ref-qc` verdicts (`PASS`/`WARN`/`FAIL`), `sample_count`, branch lists, and inferred sexes. |

### References (`reference/schema_2.0/`)

| File | Schema | Build / Mode | Key Properties | Description |
| --- | --- | --- | --- | --- |
| [`reference/schema_2.0/NIPS.GRCh38-noalt.100kb.ccnv`](reference/schema_2.0/NIPS.GRCh38-noalt.100kb.ccnv) | `2.0.0` (reference) | `GRCh38-noalt` / `prenatal` | 100 kb bins (5 kb `prenatal_ffy`), `1,060` samples, GC + mappability + fragmentomics corrected | Prenatal cfDNA NIPS reference containing `autosomal`, `female_sex`, `male_sex`, `prenatal_autosomal`, and `prenatal_ffy` branches plus `ccnv.prenatal` metadata. |
| [`reference/schema_2.0/WGS-full-pass.GRCh38-noalt.10kb.ccnv`](reference/schema_2.0/WGS-full-pass.GRCh38-noalt.10kb.ccnv) | `2.0.0` (reference) | `GRCh38-noalt` / `germline` | 10 kb bins, `79` samples, GC + mappability + fragmentomics corrected | High-coverage human WGS germline reference (`autosomal`, `female_sex`, `male_sex`). |
| [`reference/schema_2.0/WGS-low-pass.GRCh38-noalt.15kb.ccnv`](reference/schema_2.0/WGS-low-pass.GRCh38-noalt.15kb.ccnv) | `2.0.0` (reference) | `GRCh38-noalt` / `germline` | 15 kb bins, `715` samples, GC + mappability + fragmentomics corrected | Low-pass human WGS germline reference (`autosomal`, `female_sex`, `male_sex`). |
| [`reference/schema_2.0/flat_equCab-T2T.100kb.ccnv`](reference/schema_2.0/flat_equCab-T2T.100kb.ccnv) | `2.0.0` (reference) | `equCab-T2T` / `germline` (flat) | 100 kb bins, `0` cohort samples, GC + mappability corrected | Cohort-free flat FASTA reference (`autosomal` branch only, 32 equine contigs) for non-human germline testing and flat-reference `ref-qc` warning checks. |

### Predict — germline (`predict/germline/synthetic/`)

Minimal deterministic germline prediction suite (`176 KB` total) evaluated against `reference/synthetic_cohort/germline_ref_5kb.ccnv`.

| File | Schema | Layout / Properties | Description |
| --- | --- | --- | --- |
| [`predict/germline/synthetic/euploid_female.ccnv`](predict/germline/synthetic/euploid_female.ccnv) | `2.0.0` (sample) | Synthetic 6-contig, 5 kb, PE (`XX`) | Normal diploid female baseline; expects `sex="female"` and `0` calls. |
| [`predict/germline/synthetic/euploid_male.ccnv`](predict/germline/synthetic/euploid_male.ccnv) | `2.0.0` (sample) | Synthetic 6-contig, 5 kb, PE (`XY`) | Normal male baseline; expects `sex="male"` and `0` calls. |
| [`predict/germline/synthetic/cnv_events_female.ccnv`](predict/germline/synthetic/cnv_events_female.ccnv) | `2.0.0` (sample) | Synthetic 6-contig, 5 kb, PE (`XX`) | Positive CNV control carrying `CN=0` homozygous deletion (`chr1:100k–200k`), `CN=1` heterozygous deletion (`chr2:150k–250k`), and `CN=3` duplication (`chr3:200k–325k`). |
| [`predict/germline/synthetic/lowpass_homdel_male.ccnv`](predict/germline/synthetic/lowpass_homdel_male.ccnv) | `2.0.0` (sample) | Synthetic 6-contig, 5 kb, PE (~0.3x `XY`) | Ultra-low-coverage male sample with focal `CN=0` homozygous deletion (`chr1:150k–275k`), testing discrete integer copy-number Bayes factor resolution at low depth. |
| [`predict/germline/synthetic/sex_cnv_amp_male.ccnv`](predict/germline/synthetic/sex_cnv_amp_male.ccnv) | `2.0.0` (sample) | Synthetic 6-contig, 5 kb, PE (`XY`) | Male sample carrying a high-copy amplification (`CN=4`, `chr4:150k–275k`) and haploid `chrX` deletion (`CN=0`, `chrX:400k–650k`), testing `CN>=4` calling and `male_sex` branch detection. |
| [`predict/germline/synthetic/truth.json`](predict/germline/synthetic/truth.json) | JSON | — | Independent planted ground truth (`sex`, event intervals, `call`, and integer `copy_number`) for all 5 germline samples. |

### Predict — prenatal (`predict/prenatal/synthetic/`)

Minimal deterministic prenatal cfDNA prediction suite with matched 100 kb prenatal reference (`chr1`–`chr6` 2.5 Mb each, `chrX` 1.5 Mb, `chrY` 2.0 Mb).

| File | Schema | Layout / Properties | Description |
| --- | --- | --- | --- |
| [`predict/prenatal/synthetic/prenatal_ref_synthetic.ccnv`](predict/prenatal/synthetic/prenatal_ref_synthetic.ccnv) | `2.0.0` (reference) | Synthetic 8-contig / `prenatal` (`100 kb`, `30` controls) | Self-contained synthetic prenatal reference matching `NIPS.GRCh38-noalt.100kb.ccnv` branch and metadata structure. |
| [`predict/prenatal/synthetic/euploid_xx.ccnv`](predict/prenatal/synthetic/euploid_xx.ccnv) | `2.0.0` (sample) | 5 kb PE cfDNA (~10% FF, female fetus) | Euploid female fetus baseline (`fetal_sex="F"`, `sca="XX"`, `0` calls, `chrY` omitted). |
| [`predict/prenatal/synthetic/euploid_xy.ccnv`](predict/prenatal/synthetic/euploid_xy.ccnv) | `2.0.0` (sample) | 5 kb PE cfDNA (~12% FF, male fetus) | Euploid male fetus baseline (`fetal_sex="M"`, `sca="XY"`, `0` calls, haploid `chrY` emitted). |
| [`predict/prenatal/synthetic/trisomy_chr2_xy.ccnv`](predict/prenatal/synthetic/trisomy_chr2_xy.ccnv) | `2.0.0` (sample) | 5 kb PE cfDNA (~16% FF, male fetus) | Whole-chromosome `chr2` trisomy testing `FETAL_CONSTITUTIONAL` origin calling and fetal-fraction concordance. |
| [`predict/prenatal/synthetic/maternal_dup_xx.ccnv`](predict/prenatal/synthetic/maternal_dup_xx.ccnv) | `2.0.0` (sample) | 5 kb PE cfDNA (~10% FF, female fetus) | Maternal duplication (`chr3:500k–2M`, ratio ~1.5x) testing `SUPER_FETAL_RATIO` discordance and `MATERNAL_UNINHERITED` origin classification. |
| [`predict/prenatal/synthetic/twin_fm.ccnv`](predict/prenatal/synthetic/twin_fm.ccnv) | `2.0.0` (sample) | 5 kb PE cfDNA (~22% total FF: 7% M + 15% F) | Mixed-sex `FM` twin gestation testing both automatic twin detection and `--twins`, `twin.call="FM"`, and male/female fetal-fraction splitting. |
| [`predict/prenatal/synthetic/sca_xo.ccnv`](predict/prenatal/synthetic/sca_xo.ccnv) | `2.0.0` (sample) | 5 kb PE cfDNA (~14% FF, `chrX` depleted with fetal-loss fragment length, no `chrY`) | Monosomy X (`45,X` / `XO`) test case exercising 4D `erfc` ray-marginalized `XO` classification. |
| [`predict/prenatal/synthetic/chry_del_high_mw_xy.ccnv`](predict/prenatal/synthetic/chry_del_high_mw_xy.ccnv) | `2.0.0` (sample) | 5 kb PE cfDNA (~18% FF, male fetus, `high_mw_fraction > 0.05`) | Male-fetus focal `chrY` MSY microdeletion (`chrY:600k–1.0M`) paired with elevated leukocyte DNA triggering `WARN_HIGH_LEUKOCYTE_DNA`. |
| [`predict/prenatal/synthetic/truth.json`](predict/prenatal/synthetic/truth.json) | JSON | — | Independent planted ground truth (`fetal_sex`, `planted_ff`, `sca_call`, `gestation`, `twin_call`, `frag_warnings`, and planted event intervals/discordance/`origin_call`). |

### Predict — prenatal twins (`predict/prenatal/twins/`)

Anonymized real-world paired-end cfDNA twin gestation sketches (`12` samples, `GRCh38-noalt`, `5 kb` bins, `6.4M`–`20.7M` kept reads) paired with `reference/schema_2.0/NIPS.GRCh38-noalt.100kb.ccnv`. All internal paths and clinical identifiers have been stripped.

| File | Schema | Layout / Properties | Description |
| --- | --- | --- | --- |
| [`predict/prenatal/twins/twin_ff_01.ccnv`](predict/prenatal/twins/twin_ff_01.ccnv) | `2.0.0` (sample) | `GRCh38-noalt`, 5 kb, PE, `14.37M` kept reads | Confirmed `FF` (`female`/`female`) twin pregnancy testing concordant female-twin stoichiometry and `chrY` omission. |
| [`predict/prenatal/twins/twin_ff_02.ccnv`](predict/prenatal/twins/twin_ff_02.ccnv) | `2.0.0` (sample) | `GRCh38-noalt`, 5 kb, PE, `13.92M` kept reads | Confirmed `FF` (`female`/`female`) twin pregnancy. |
| [`predict/prenatal/twins/twin_ff_03.ccnv`](predict/prenatal/twins/twin_ff_03.ccnv) | `2.0.0` (sample) | `GRCh38-noalt`, 5 kb, PE, `16.86M` kept reads | Confirmed `FF` (`female`/`female`) twin pregnancy. |
| [`predict/prenatal/twins/twin_fm_01.ccnv`](predict/prenatal/twins/twin_fm_01.ccnv) | `2.0.0` (sample) | `GRCh38-noalt`, 5 kb, PE, `12.90M` kept reads | Confirmed `FM` (`female`/`male`) mixed-sex twin pregnancy testing `male_ff`/`female_ff` split and `chrY` anchoring. |
| [`predict/prenatal/twins/twin_fm_02.ccnv`](predict/prenatal/twins/twin_fm_02.ccnv) | `2.0.0` (sample) | `GRCh38-noalt`, 5 kb, PE, `6.42M` kept reads | Confirmed `FM` (`female`/`male`) mixed-sex twin pregnancy at lower sequencing depth (`6.4M` reads). |
| [`predict/prenatal/twins/twin_fm_03.ccnv`](predict/prenatal/twins/twin_fm_03.ccnv) | `2.0.0` (sample) | `GRCh38-noalt`, 5 kb, PE, `20.70M` kept reads | Confirmed `FM` (`female`/`male`) mixed-sex twin pregnancy at higher sequencing depth (`20.7M` reads). |
| [`predict/prenatal/twins/twin_fm_04.ccnv`](predict/prenatal/twins/twin_fm_04.ccnv) | `2.0.0` (sample) | `GRCh38-noalt`, 5 kb, PE, `11.45M` kept reads | Confirmed `FM` (`female`/`male`) mixed-sex twin pregnancy. |
| [`predict/prenatal/twins/twin_mm_01.ccnv`](predict/prenatal/twins/twin_mm_01.ccnv) | `2.0.0` (sample) | `GRCh38-noalt`, 5 kb, PE, `13.94M` kept reads | Confirmed `MM` (`male`/`male`) twin pregnancy testing concordant male-twin stoichiometry and haploid `chrY` normalization. |
| [`predict/prenatal/twins/twin_mm_02.ccnv`](predict/prenatal/twins/twin_mm_02.ccnv) | `2.0.0` (sample) | `GRCh38-noalt`, 5 kb, PE, `12.74M` kept reads | Confirmed `MM` (`male`/`male`) twin pregnancy. |
| [`predict/prenatal/twins/twin_mm_03.ccnv`](predict/prenatal/twins/twin_mm_03.ccnv) | `2.0.0` (sample) | `GRCh38-noalt`, 5 kb, PE, `14.17M` kept reads | Confirmed `MM` (`male`/`male`) twin pregnancy. |
| [`predict/prenatal/twins/twin_mm_04.ccnv`](predict/prenatal/twins/twin_mm_04.ccnv) | `2.0.0` (sample) | `GRCh38-noalt`, 5 kb, PE, `17.46M` kept reads | Confirmed `MM` (`male`/`male`) twin pregnancy. |
| [`predict/prenatal/twins/twin_mm_05.ccnv`](predict/prenatal/twins/twin_mm_05.ccnv) | `2.0.0` (sample) | `GRCh38-noalt`, 5 kb, PE, `14.49M` kept reads | Confirmed `MM` (`male`/`male`) twin pregnancy. |
| [`predict/prenatal/twins/truth.json`](predict/prenatal/twins/truth.json) | JSON | — | Anonymized ground-truth manifest recording expected twin sex pair (`FF`/`FM`/`MM`), fetal sexes, bin size, and read counts. |

### Predict — prenatal SCAs (`predict/prenatal/sca/`)

Anonymized real-world paired-end cfDNA sex chromosome aneuploidy (SCA) sketches (`3` samples, `GRCh38-noalt`, `5 kb` bins, ~`13M` kept reads) paired with `reference/schema_2.0/NIPS.GRCh38-noalt.100kb.ccnv`. All internal paths and clinical identifiers have been stripped.

| File | Schema | Layout / Properties | Description |
| --- | --- | --- | --- |
| [`predict/prenatal/sca/sca_xo_01.ccnv`](predict/prenatal/sca/sca_xo_01.ccnv) | `2.0.0` (sample) | `GRCh38-noalt`, 5 kb, PE, `13.01M` kept reads | Confirmed Monosomy X (`45,X` / `XO`) singleton testing 4D `erfc` ray-marginalized SCA classification on real cfDNA. |
| [`predict/prenatal/sca/sca_xxx_01.ccnv`](predict/prenatal/sca/sca_xxx_01.ccnv) | `2.0.0` (sample) | `GRCh38-noalt`, 5 kb, PE, `13.13M` kept reads | Confirmed Triple X (`47,XXX` / `XXX`) singleton testing `chrX` gain SCA classification. |
| [`predict/prenatal/sca/sca_xxy_01.ccnv`](predict/prenatal/sca/sca_xxy_01.ccnv) | `2.0.0` (sample) | `GRCh38-noalt`, 5 kb, PE, `13.03M` kept reads | Confirmed Klinefelter (`47,XXY` / `XXY`) singleton testing male-fetus `chrX` excess SCA classification. |
| [`predict/prenatal/sca/truth.json`](predict/prenatal/sca/truth.json) | JSON | — | Anonymized ground-truth manifest recording expected SCA class (`XO`/`XXX`/`XXY`), karyotype, fetal sex, bin size, and read counts. |

### Real-world data gaps (needed)

Synthetic fixtures above cover mechanical pipeline behavior, formats, tracks, `ref-qc` verdicts, and baseline calling. The following real-world `5 kb` schema `2.0.0` `.ccnv` datasets (plus ground-truth manifests, stripped of internal paths and clinical IDs) are still needed to validate empirical biological noise, GC/mappability/fragmentomics structure, and clinical edge cases against the real references in `reference/schema_2.0/`:

| Target directory | Paired reference | Missing real-world dataset(s) | Behavior / edge case validated |
| --- | --- | --- | --- |
| `predict/germline/` | `WGS-low-pass.GRCh38-noalt.15kb.ccnv` | 1 euploid female (`XX`) + 1 euploid male (`XY`) low-pass (~0.1x–1x) sample | Baseline false-positive rate and automatic sex branch routing at low coverage. |
| `predict/germline/` | `WGS-full-pass.GRCh38-noalt.10kb.ccnv`, `WGS-low-pass.GRCh38-noalt.15kb.ccnv` | Confirmed focal CNVs: low-pass `CN=0` homozygous deletion, `CN=1` heterozygous deletion, `CN=3` duplication, `CN>=4` amplification, and a `chrX`/`chrY` event | Discrete integer copy-number Bayes factor resolution across copy states and sex branches. |
| `predict/germline/` | `WGS-full-pass.GRCh38-noalt.10kb.ccnv` | CNV flanking or spanning a real GRCh38 centromeric gap (`>2 Mb`) | Euchromatic arm-block splitting (`--max-blacklist-gap`) on real repeat architecture. |
| `predict/germline/` | `flat_equCab-T2T.100kb.ccnv` | 1 positive non-human (`equCab-T2T`) CNV or aneuploidy sample | End-to-end non-human event detection against a cohort-free flat FASTA reference. |
| `predict/prenatal/` | `NIPS.GRCh38-noalt.100kb.ccnv` | Euploid `XX` and `XY` singletons across low (~3–4%), mid (~10%), and high (~20%) FF, plus 1 high-leukocyte-DNA (`high_mw_fraction > 0.05`) sample | Real-cfDNA `FFY`/`FFX`/`FFA`/`FF_size` Student-t4 fusion, `chrY` omission (`F`) vs haploid calling (`M`), and `WARN_HIGH_LEUKOCYTE_DNA`. |
| `predict/prenatal/` | `NIPS.GRCh38-noalt.100kb.ccnv` | Confirmed autosomal trisomies: T21 (including 1 borderline ~4% FF case), T18, and T13 | Real-world sensitivity, analytical LoD accuracy, and fetal-fraction concordance. |
| `predict/prenatal/sca/` | `NIPS.GRCh38-noalt.100kb.ccnv` | Remaining sex chromosome aneuploidies: `XYY` (`47,XYY`) plus 1 maternal SCA / `DISCORDANT_MIXED` case (`XO`, `XXX`, and `XXY` already covered) | Full 7-state 4D `erfc` ray-marginalized SCA classifier coverage on real cfDNA. |
| `predict/prenatal/` | `NIPS.GRCh38-noalt.100kb.ccnv` | Biological origin cases: `FETAL_CONSTITUTIONAL` microdeletion/duplication (e.g. 22q11.2), `PLACENTAL_MOSAIC` (CPM), `MATERNAL_UNINHERITED` / `MATERNAL_AND_FETAL_INHERITED` maternal CNV, and a male-fetus `chrY` MSY deletion/gain | 5-state coupled depth + Dirichlet-Multinomial origin engine, sizing-shift alignment, and haploid `prenatal_ffy` `chrY` calling. |
| `tracks/` (optional) | `reference/schema_2.0/*` | Real GRCh38 / `equCab-T2T` GC, mappability BigWig, and non-hg38 cytoband tracks | Real-scale covariate re-normalization benchmarks and non-hg38 `--cytobands` plotting. |
