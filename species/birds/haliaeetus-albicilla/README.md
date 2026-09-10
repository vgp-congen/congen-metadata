<!-- congen:begin body -->
# *Haliaeetus albicilla*

Population-genomic variant calls for 96 *Haliaeetus albicilla* samples ([NCBI taxon 8969](https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=8969)), produced by snpArcher against `GCA_947461875.1` and published on GenomeArk.

> [!NOTE]
> **PASS WITH WARNINGS** · validated 2026-09-10 · `GCA_947461875.1`
>
> Metadata agrees with the data published on GenomeArk, with 1 warning to note.
>
> **Provenance incomplete** — no reviewed list of contributing BioProjects exists for this species, so it cannot yet be cited. (`G017`)
>
> See [References](#references).
>
> Full report: [`VALIDATION.md`](VALIDATION.md) · check definitions: [`CHECKS.md`](../../../CHECKS.md) · baseline description: [`README.txt`](README.txt)

## Dataset

|  |  |
|---|---|
| Clade | birds |
| Reference assembly | [`GCA_947461875.1`](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_947461875.1/) — bHalAlb1.1, Chromosome |
| Variant caller | gatk 4.6.2.0 |
| Samples | 96 across 97 sheet rows, all from SRA runs |
| Variant sites | 12,931,023 |

<details>
<summary>Assembly and pipeline detail</summary>

|  |  |
|---|---|
| Assembly organism | Haliaeetus albicilla |
| Paired RefSeq/GenBank accession | `GCF_947461875.1` |
| NCBI taxon | 8969 |
| Contigs in the VCF | 188 |
| Ploidy / heterozygosity prior | 2 / 0.005 |
| Other tools recorded in the VCF header | bcftools 1.23.1 |

</details>

## Sample QC

Full plots: [the snpArcher QC dashboard][qc/qc_dashboard.html]. Per-sample values are not reproduced here; they are in [`qc_report.tsv`][qc/qc_report.tsv], [`individuals.het`][qc/individuals.het] and [`individuals.imiss`][qc/individuals.imiss].

**Mean depth 19.4×** (IQR 13.2–24.4, range 0.1× `SAMEA117070514` to 37.7× `SAMEA117070573`), over mapped reads.

| mean depth | samples |  |
|---|---:|---|
| 0–5× | 6 | █████ |
| 5–10× | 8 | ██████ |
| 10–15× | 15 | ████████████ |
| 15–20× | 23 | ██████████████████ |
| 20–30× | 39 | ██████████████████████████████ |
| 30–40× | 5 | ████ |

Across the cohort:

|  |  |
|---|---|
| Cohort mean coverage | 18.4× |
| Reads mapped | median 99.5% (range 2.3–99.7%) |
| Duplicates | median 16.8% (range 0.4–25.8%) |
| Properly paired | median 98.1% (range 1.0–99.2%) |
| Missingness F_MISS | median 0.152 (range 0.097–0.963) |
| Inbreeding coefficient F | median 0.069 (range -0.777–0.974) |

The callable-sites mask was built over depths 9–37× ([`coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv]). That is a site-level threshold for the mask, not a per-sample cutoff: samples outside it are neither excluded nor flagged here.

## References

> [!WARNING]
> **This dataset cannot yet be cited.**
>
> No reviewed list of contributing BioProjects exists in this repository, so there is nothing here to credit the people who generated the reads. Do not publish analyses of this dataset until that list exists.
>
> Reported by `G017` — see [`VALIDATION.md`](VALIDATION.md) for what that means and how to fix it.

## Getting the data

Everything below is under `s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_947461875.1/`. The bucket is public and needs no credentials, but the AWS CLI needs `--no-sign-request` or it will try to sign the request and fail.

|  | Size |  |
|---|---:|---|
| [`raw.vcf.gz`][vcfs/raw.vcf.gz] | 6.6 GiB | unfiltered joint-genotyped calls |
| [`raw.vcf.gz.tbi`][vcfs/raw.vcf.gz.tbi] | 1.1 MiB | index for the above |
| [`callable_sites.bed`][callable_sites/callable_sites.bed] | 1.6 MiB | the callable-sites mask |
| [`qc_dashboard.html`][qc/qc_dashboard.html] | 10.4 MiB | the snpArcher QC report — opens in a browser |
| `bams/` | 1006.6 GiB | 192 objects — alignments and indexes |

This run did not produce `filtered.vcf.gz`.

```bash
# one region of the VCF, without downloading the whole thing
bcftools view -r <chr>:<start>-<end> \
  https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_947461875.1/vcfs/raw.vcf.gz

# the alignments — check the size above first
aws s3 sync --no-sign-request \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_947461875.1/bams/ ./bams/

# the masks and QC tables, without the zarr stores
aws s3 sync --no-sign-request --exclude '*.zarr/*' \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_947461875.1/callable_sites/ ./callable_sites/
```

> [!WARNING]
> `callable_sites/` also holds `callable_loci.zarr/`, `depths/`, `depths.zarr/` and `genmap_index/` — zarr stores of thousands of small objects each.
>
> Nothing here lists or sizes them, so the sizes above are a lower bound, and a recursive `sync` without the `--exclude` above will pull all of them.

<details>
<summary>The other 10 published files</summary>

|  | Size |
|---|---:|
| [`qc/qc_report.tsv`][qc/qc_report.tsv] | 9.1 KiB |
| [`qc/individuals.idepth`][qc/individuals.idepth] | 3.0 KiB |
| [`qc/individuals.het`][qc/individuals.het] | 4.7 KiB |
| [`qc/individuals.imiss`][qc/individuals.imiss] | 4.1 KiB |
| [`qc/individuals.samps.txt`][qc/individuals.samps.txt] | 1.4 KiB |
| [`qc/contig_map.tsv`][qc/contig_map.tsv] | 6.8 KiB |
| [`callable_sites/coverage.bed`][callable_sites/coverage.bed] | 0 B |
| [`callable_sites/mappability.bed`][callable_sites/mappability.bed] | 1.6 MiB |
| [`callable_sites/mappability.bedgraph`][callable_sites/mappability.bedgraph] | 1.8 GiB |
| [`callable_sites/coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv] | 62 B |

</details>

[callable_sites/callable_sites.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_947461875.1/callable_sites/callable_sites.bed
[callable_sites/coverage.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_947461875.1/callable_sites/coverage.bed
[callable_sites/coverage_thresholds.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_947461875.1/callable_sites/coverage_thresholds.tsv
[callable_sites/mappability.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_947461875.1/callable_sites/mappability.bed
[callable_sites/mappability.bedgraph]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_947461875.1/callable_sites/mappability.bedgraph
[qc/contig_map.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_947461875.1/qc/contig_map.tsv
[qc/individuals.het]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_947461875.1/qc/individuals.het
[qc/individuals.idepth]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_947461875.1/qc/individuals.idepth
[qc/individuals.imiss]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_947461875.1/qc/individuals.imiss
[qc/individuals.samps.txt]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_947461875.1/qc/individuals.samps.txt
[qc/qc_dashboard.html]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_947461875.1/qc/qc_dashboard.html
[qc/qc_report.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_947461875.1/qc/qc_report.tsv
[vcfs/raw.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_947461875.1/vcfs/raw.vcf.gz
[vcfs/raw.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_947461875.1/vcfs/raw.vcf.gz.tbi
<!-- congen:end body -->
