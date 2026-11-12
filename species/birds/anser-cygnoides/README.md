<!-- congen:begin body -->
# *Anser cygnoides*

Population-genomic variant calls for 302 *Anser cygnoides* samples ([NCBI taxon 8845](https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=8845)), produced by snpArcher against `GCA_040182565.1` and published on GenomeArk.

> [!NOTE]
> **PASS WITH WARNINGS** · validated 2026-11-12 · `GCA_040182565.1`
>
> Metadata agrees with the data published on GenomeArk, with 1 warning to note.
>
> **Provenance incorrect** — the recorded list of contributing BioProjects is known to be wrong, so it cannot be used for citation. (`E002`)
>
> See [References](#references).
>
> Full report: [`VALIDATION.md`](VALIDATION.md) · check definitions: [`CHECKS.md`](../../../CHECKS.md) · baseline description: [`README.txt`](README.txt)

## Dataset

|  |  |
|---|---|
| Clade | birds |
| Reference assembly | [`GCA_040182565.1`](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_040182565.1/) — Taihu_goose_T2T_genome, Chromosome |
| Variant caller | gatk 4.6.2.0 |
| Samples | 302 across 331 sheet rows, all from SRA runs |
| Variant sites | 154,075,645 |

<details>
<summary>Assembly and pipeline detail</summary>

|  |  |
|---|---|
| Assembly organism | Anser cygnoides |
| Paired RefSeq/GenBank accession | `GCF_040182565.1` |
| NCBI taxon | 8845 |
| Contigs in the VCF | 74 |
| Ploidy / heterozygosity prior | 2 / 0.005 |
| Other tools recorded in the VCF header | bcftools 1.24 |

</details>

## Sample QC

Full plots: [the snpArcher QC dashboard][qc/qc_dashboard.html]. Per-sample values are not reproduced here; they are in [`qc_report.tsv`][qc/qc_report.tsv], [`individuals.het`][qc/individuals.het] and [`individuals.imiss`][qc/individuals.imiss].

**Mean depth 10.7×** (IQR 8.3–14.6, range 6.7× `SAMN36175415` to 82.1× `SAMN19666339`), over mapped reads.

| mean depth | samples |  |
|---|---:|---|
| 5–10× | 128 | ██████████████████████████████ |
| 10–15× | 100 | ███████████████████████ |
| 15–20× | 4 | █ |
| 20–30× | 52 | ████████████ |
| 30–40× | 16 | ████ |
| 40–60× | 1 | █ |
| 60–100× | 1 | █ |

Across the cohort:

|  |  |
|---|---|
| Cohort mean coverage | 13.1× |
| Reads mapped | median 99.7% (range 76.0–99.8%) |
| Duplicates | median 11.1% (range 0.0–36.0%) |
| Properly paired | median 98.3% (range 66.3–99.1%) |
| Missingness F_MISS | median 0.033 (range 0.013–0.524) |
| Inbreeding coefficient F | median 0.483 (range -3.656–0.742) |

The callable-sites mask was built over depths 6–27× ([`coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv]). That is a site-level threshold for the mask, not a per-sample cutoff: samples outside it are neither excluded nor flagged here.

## References

> [!CAUTION]
> **This dataset cannot yet be cited.**
>
> The recorded list of contributing BioProjects is known to be incorrect, so it is not reproduced here — a wrong citation list is worse than none, because it propagates into other people's papers. Do not publish analyses of this dataset until it is corrected.
>
> Reported by `E002` — see [`VALIDATION.md`](VALIDATION.md) for what that means and how to fix it.

## Getting the data

Everything below is under `s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_040182565.1/`. The bucket is public and needs no credentials, but the AWS CLI needs `--no-sign-request` or it will try to sign the request and fail.

|  | Size |  |
|---|---:|---|
| [`raw.vcf.gz`][vcfs/raw.vcf.gz] | 152.7 GiB | unfiltered joint-genotyped calls |
| [`raw.vcf.gz.tbi`][vcfs/raw.vcf.gz.tbi] | 1.1 MiB | index for the above |
| [`filtered.vcf.gz`][vcfs/filtered.vcf.gz] | 249.4 GiB | filtered calls |
| [`filtered.vcf.gz.tbi`][vcfs/filtered.vcf.gz.tbi] | 1.1 MiB | index for the above |
| [`callable_sites.bed`][callable_sites/callable_sites.bed] | 1.9 MiB | the callable-sites mask |
| [`qc_dashboard.html`][qc/qc_dashboard.html] | 20.9 MiB | the snpArcher QC report — opens in a browser |
| `bams/` | 2590.9 GiB | 604 objects — alignments and indexes |

```bash
# one region of the VCF, without downloading the whole thing
bcftools view -r <chr>:<start>-<end> \
  https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_040182565.1/vcfs/raw.vcf.gz

# the alignments — check the size above first
aws s3 sync --no-sign-request \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_040182565.1/bams/ ./bams/

# the masks and QC tables, without the zarr stores
aws s3 sync --no-sign-request --exclude '*.zarr/*' \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_040182565.1/callable_sites/ ./callable_sites/
```

> [!WARNING]
> `callable_sites/` also holds `callable_loci.zarr/`, `depths/`, `depths.zarr/` and `genmap_index/` — zarr stores of thousands of small objects each.
>
> Nothing here lists or sizes them, so the sizes above are a lower bound, and a recursive `sync` without the `--exclude` above will pull all of them.

<details>
<summary>The other 10 published files</summary>

|  | Size |
|---|---:|
| [`qc/qc_report.tsv`][qc/qc_report.tsv] | 27.0 KiB |
| [`qc/individuals.idepth`][qc/individuals.idepth] | 9.1 KiB |
| [`qc/individuals.het`][qc/individuals.het] | 15.6 KiB |
| [`qc/individuals.imiss`][qc/individuals.imiss] | 12.7 KiB |
| [`qc/individuals.samps.txt`][qc/individuals.samps.txt] | 3.8 KiB |
| [`qc/contig_map.tsv`][qc/contig_map.tsv] | 2.3 KiB |
| [`callable_sites/coverage.bed`][callable_sites/coverage.bed] | 0 B |
| [`callable_sites/mappability.bed`][callable_sites/mappability.bed] | 1.9 MiB |
| [`callable_sites/mappability.bedgraph`][callable_sites/mappability.bedgraph] | 810.7 MiB |
| [`callable_sites/coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv] | 62 B |

</details>

[callable_sites/callable_sites.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_040182565.1/callable_sites/callable_sites.bed
[callable_sites/coverage.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_040182565.1/callable_sites/coverage.bed
[callable_sites/coverage_thresholds.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_040182565.1/callable_sites/coverage_thresholds.tsv
[callable_sites/mappability.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_040182565.1/callable_sites/mappability.bed
[callable_sites/mappability.bedgraph]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_040182565.1/callable_sites/mappability.bedgraph
[qc/contig_map.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_040182565.1/qc/contig_map.tsv
[qc/individuals.het]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_040182565.1/qc/individuals.het
[qc/individuals.idepth]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_040182565.1/qc/individuals.idepth
[qc/individuals.imiss]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_040182565.1/qc/individuals.imiss
[qc/individuals.samps.txt]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_040182565.1/qc/individuals.samps.txt
[qc/qc_dashboard.html]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_040182565.1/qc/qc_dashboard.html
[qc/qc_report.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_040182565.1/qc/qc_report.tsv
[vcfs/filtered.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_040182565.1/vcfs/filtered.vcf.gz
[vcfs/filtered.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_040182565.1/vcfs/filtered.vcf.gz.tbi
[vcfs/raw.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_040182565.1/vcfs/raw.vcf.gz
[vcfs/raw.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_040182565.1/vcfs/raw.vcf.gz.tbi
<!-- congen:end body -->
