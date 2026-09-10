<!-- congen:begin body -->
# *Dryobates pubescens*

Population-genomic variant calls for 21 *Dryobates pubescens* samples ([NCBI taxon 118200](https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=118200)), produced by snpArcher against `GCA_014839835.1` and published on GenomeArk.

> [!NOTE]
> **PASS WITH WARNINGS** · validated 2026-09-10 · `GCA_014839835.1`
>
> Metadata agrees with the data published on GenomeArk, with 2 warnings to note.
>
> **Provenance incorrect** — the recorded list of contributing BioProjects is known to be wrong, so it cannot be used for citation. (`E002` and `E003`)
>
> See [References](#references).
>
> Full report: [`VALIDATION.md`](VALIDATION.md) · check definitions: [`CHECKS.md`](../../../CHECKS.md) · baseline description: [`README.txt`](README.txt)

## Dataset

|  |  |
|---|---|
| Clade | birds |
| Reference assembly | [`GCA_014839835.1`](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_014839835.1/) — bDryPub1.pri, Chromosome |
| Variant caller | gatk 4.6.2.0 |
| Samples | 21 across 22 sheet rows, all from SRA runs |
| Variant sites | 40,940,451 |

<details>
<summary>Assembly and pipeline detail</summary>

|  |  |
|---|---|
| Assembly organism | Dryobates pubescens |
| Paired RefSeq/GenBank accession | `GCF_014839835.1` |
| NCBI taxon | 118200 |
| Contigs in the VCF | 181 |
| Ploidy / heterozygosity prior | 2 / 0.005 |
| Other tools recorded in the VCF header | bcftools 1.23.1 |

</details>

## Sample QC

Full plots: [the snpArcher QC dashboard][qc/qc_dashboard.html]. Per-sample values are not reproduced here; they are in [`qc_report.tsv`][qc/qc_report.tsv], [`individuals.het`][qc/individuals.het] and [`individuals.imiss`][qc/individuals.imiss].

**Mean depth 19.2×** (IQR 10.4–21.5, range 8.8× `SAMN32543631` to 29.3× `SAMN48575285`), over mapped reads.

| mean depth | samples |  |
|---|---:|---|
| 5–10× | 3 | ███████████████ |
| 10–15× | 6 | ██████████████████████████████ |
| 15–20× | 6 | ██████████████████████████████ |
| 20–30× | 6 | ██████████████████████████████ |

Across the cohort:

|  |  |
|---|---|
| Cohort mean coverage | 16.0× |
| Reads mapped | median 99.2% (range 98.4–99.5%) |
| Duplicates | median 10.4% (range 4.5–17.3%) |
| Properly paired | median 96.8% (range 91.3–97.5%) |
| Missingness F_MISS | median 0.034 (range 0.023–0.192) |
| Inbreeding coefficient F | median 0.073 (range -0.135–0.284) |

The callable-sites mask was built over depths 8–33× ([`coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv]). That is a site-level threshold for the mask, not a per-sample cutoff: samples outside it are neither excluded nor flagged here.

## References

> [!CAUTION]
> **This dataset cannot yet be cited.**
>
> The recorded list of contributing BioProjects is known to be incorrect, so it is not reproduced here — a wrong citation list is worse than none, because it propagates into other people's papers. Do not publish analyses of this dataset until it is corrected.
>
> Reported by `E002` and `E003` — see [`VALIDATION.md`](VALIDATION.md) for what these mean and how to fix it.

## Getting the data

Everything below is under `s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_014839835.1/`. The bucket is public and needs no credentials, but the AWS CLI needs `--no-sign-request` or it will try to sign the request and fail.

|  | Size |  |
|---|---:|---|
| [`raw.vcf.gz`][vcfs/raw.vcf.gz] | 6.7 GiB | unfiltered joint-genotyped calls |
| [`raw.vcf.gz.tbi`][vcfs/raw.vcf.gz.tbi] | 1.0 MiB | index for the above |
| [`callable_sites.bed`][callable_sites/callable_sites.bed] | 3.8 MiB | the callable-sites mask |
| [`qc_dashboard.html`][qc/qc_dashboard.html] | 8.6 MiB | the snpArcher QC report — opens in a browser |
| `bams/` | 202.5 GiB | 42 objects — alignments and indexes |

This run did not produce `filtered.vcf.gz`.

```bash
# one region of the VCF, without downloading the whole thing
bcftools view -r <chr>:<start>-<end> \
  https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_014839835.1/vcfs/raw.vcf.gz

# the alignments — check the size above first
aws s3 sync --no-sign-request \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_014839835.1/bams/ ./bams/

# the masks and QC tables, without the zarr stores
aws s3 sync --no-sign-request --exclude '*.zarr/*' \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_014839835.1/callable_sites/ ./callable_sites/
```

> [!WARNING]
> `callable_sites/` also holds `callable_loci.zarr/`, `depths/`, `depths.zarr/` and `genmap_index/` — zarr stores of thousands of small objects each.
>
> Nothing here lists or sizes them, so the sizes above are a lower bound, and a recursive `sync` without the `--exclude` above will pull all of them.

<details>
<summary>The other 10 published files</summary>

|  | Size |
|---|---:|
| [`qc/qc_report.tsv`][qc/qc_report.tsv] | 2.1 KiB |
| [`qc/individuals.idepth`][qc/individuals.idepth] | 654 B |
| [`qc/individuals.het`][qc/individuals.het] | 1.1 KiB |
| [`qc/individuals.imiss`][qc/individuals.imiss] | 924 B |
| [`qc/individuals.samps.txt`][qc/individuals.samps.txt] | 273 B |
| [`qc/contig_map.tsv`][qc/contig_map.tsv] | 6.4 KiB |
| [`callable_sites/coverage.bed`][callable_sites/coverage.bed] | 36.9 MiB |
| [`callable_sites/mappability.bed`][callable_sites/mappability.bed] | 3.0 MiB |
| [`callable_sites/mappability.bedgraph`][callable_sites/mappability.bedgraph] | 563.2 MiB |
| [`callable_sites/coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv] | 62 B |

</details>

[callable_sites/callable_sites.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_014839835.1/callable_sites/callable_sites.bed
[callable_sites/coverage.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_014839835.1/callable_sites/coverage.bed
[callable_sites/coverage_thresholds.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_014839835.1/callable_sites/coverage_thresholds.tsv
[callable_sites/mappability.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_014839835.1/callable_sites/mappability.bed
[callable_sites/mappability.bedgraph]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_014839835.1/callable_sites/mappability.bedgraph
[qc/contig_map.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_014839835.1/qc/contig_map.tsv
[qc/individuals.het]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_014839835.1/qc/individuals.het
[qc/individuals.idepth]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_014839835.1/qc/individuals.idepth
[qc/individuals.imiss]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_014839835.1/qc/individuals.imiss
[qc/individuals.samps.txt]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_014839835.1/qc/individuals.samps.txt
[qc/qc_dashboard.html]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_014839835.1/qc/qc_dashboard.html
[qc/qc_report.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_014839835.1/qc/qc_report.tsv
[vcfs/raw.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_014839835.1/vcfs/raw.vcf.gz
[vcfs/raw.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_014839835.1/vcfs/raw.vcf.gz.tbi
<!-- congen:end body -->
