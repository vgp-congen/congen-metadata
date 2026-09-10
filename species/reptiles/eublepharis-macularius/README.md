<!-- congen:begin body -->
# *Eublepharis macularius*

Population-genomic variant calls for 26 *Eublepharis macularius* samples ([NCBI taxon 481883](https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=481883)), produced by snpArcher against `GCA_028583425.1` and published on GenomeArk.

> [!NOTE]
> **PASS WITH WARNINGS** · validated 2026-09-10 · `GCA_028583425.1`
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
| Clade | reptiles |
| Reference assembly | [`GCA_028583425.1`](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_028583425.1/) — MPM_Emac_v1.0, Chromosome |
| Variant caller | gatk 4.6.2.0 |
| Samples | 26 across 29 sheet rows, all from SRA runs |
| Variant sites | 33,627,754 |

<details>
<summary>Assembly and pipeline detail</summary>

|  |  |
|---|---|
| Assembly organism | Eublepharis macularius |
| Paired RefSeq/GenBank accession | `GCF_028583425.1` |
| NCBI taxon | 481883 |
| Contigs in the VCF | 75 |
| Ploidy / heterozygosity prior | 2 / 0.005 |
| Other tools recorded in the VCF header | bcftools 1.23.1 |

</details>

## Sample QC

Full plots: [the snpArcher QC dashboard][qc/qc_dashboard.html]. Per-sample values are not reproduced here; they are in [`qc_report.tsv`][qc/qc_report.tsv], [`individuals.het`][qc/individuals.het] and [`individuals.imiss`][qc/individuals.imiss].

**Mean depth 22.1×** (IQR 18.5–24.0, range 0.7× `SAMN19217408` to 43.6× `SAMN19217406`), over mapped reads.

| mean depth | samples |  |
|---|---:|---|
| 0–5× | 1 | ██ |
| 5–10× | 0 |  |
| 10–15× | 2 | ████ |
| 15–20× | 5 | ███████████ |
| 20–30× | 14 | ██████████████████████████████ |
| 30–40× | 2 | ████ |
| 40–60× | 2 | ████ |

Across the cohort:

|  |  |
|---|---|
| Cohort mean coverage | 21.1× |
| Reads mapped | median 99.7% (range 99.2–99.8%) |
| Duplicates | median 17.3% (range 2.5–94.5%) |
| Properly paired | median 97.2% (range 89.1–98.3%) |
| Missingness F_MISS | median 0.038 (range 0.027–0.937) |
| Inbreeding coefficient F | median -0.369 (range -0.937–0.616) |

The callable-sites mask was built over depths 10–43× ([`coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv]). That is a site-level threshold for the mask, not a per-sample cutoff: samples outside it are neither excluded nor flagged here.

## References

> [!WARNING]
> **This dataset cannot yet be cited.**
>
> No reviewed list of contributing BioProjects exists in this repository, so there is nothing here to credit the people who generated the reads. Do not publish analyses of this dataset until that list exists.
>
> Reported by `G017` — see [`VALIDATION.md`](VALIDATION.md) for what that means and how to fix it.

## Getting the data

Everything below is under `s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_028583425.1/`. The bucket is public and needs no credentials, but the AWS CLI needs `--no-sign-request` or it will try to sign the request and fail.

|  | Size |  |
|---|---:|---|
| [`raw.vcf.gz`][vcfs/raw.vcf.gz] | 7.3 GiB | unfiltered joint-genotyped calls |
| [`raw.vcf.gz.tbi`][vcfs/raw.vcf.gz.tbi] | 2.0 MiB | index for the above |
| [`filtered.vcf.gz`][vcfs/filtered.vcf.gz] | 9.4 GiB | filtered calls |
| [`filtered.vcf.gz.tbi`][vcfs/filtered.vcf.gz.tbi] | 2.0 MiB | index for the above |
| [`callable_sites.bed`][callable_sites/callable_sites.bed] | 9.8 MiB | the callable-sites mask |
| [`qc_dashboard.html`][qc/qc_dashboard.html] | 8.7 MiB | the snpArcher QC report — opens in a browser |
| `bams/` | 673.7 GiB | 52 objects — alignments and indexes |

```bash
# one region of the VCF, without downloading the whole thing
bcftools view -r <chr>:<start>-<end> \
  https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_028583425.1/vcfs/raw.vcf.gz

# the alignments — check the size above first
aws s3 sync --no-sign-request \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_028583425.1/bams/ ./bams/

# the masks and QC tables, without the zarr stores
aws s3 sync --no-sign-request --exclude '*.zarr/*' \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_028583425.1/callable_sites/ ./callable_sites/
```

> [!WARNING]
> `callable_sites/` also holds `callable_loci.zarr/`, `depths/`, `depths.zarr/` and `genmap_index/` — zarr stores of thousands of small objects each.
>
> Nothing here lists or sizes them, so the sizes above are a lower bound, and a recursive `sync` without the `--exclude` above will pull all of them.

<details>
<summary>The other 10 published files</summary>

|  | Size |
|---|---:|
| [`qc/qc_report.tsv`][qc/qc_report.tsv] | 2.6 KiB |
| [`qc/individuals.idepth`][qc/individuals.idepth] | 801 B |
| [`qc/individuals.het`][qc/individuals.het] | 1.3 KiB |
| [`qc/individuals.imiss`][qc/individuals.imiss] | 1.1 KiB |
| [`qc/individuals.samps.txt`][qc/individuals.samps.txt] | 338 B |
| [`qc/contig_map.tsv`][qc/contig_map.tsv] | 2.6 KiB |
| [`callable_sites/coverage.bed`][callable_sites/coverage.bed] | 3.0 MiB |
| [`callable_sites/mappability.bed`][callable_sites/mappability.bed] | 9.7 MiB |
| [`callable_sites/mappability.bedgraph`][callable_sites/mappability.bedgraph] | 2.6 GiB |
| [`callable_sites/coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv] | 63 B |

</details>

[callable_sites/callable_sites.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_028583425.1/callable_sites/callable_sites.bed
[callable_sites/coverage.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_028583425.1/callable_sites/coverage.bed
[callable_sites/coverage_thresholds.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_028583425.1/callable_sites/coverage_thresholds.tsv
[callable_sites/mappability.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_028583425.1/callable_sites/mappability.bed
[callable_sites/mappability.bedgraph]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_028583425.1/callable_sites/mappability.bedgraph
[qc/contig_map.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_028583425.1/qc/contig_map.tsv
[qc/individuals.het]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_028583425.1/qc/individuals.het
[qc/individuals.idepth]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_028583425.1/qc/individuals.idepth
[qc/individuals.imiss]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_028583425.1/qc/individuals.imiss
[qc/individuals.samps.txt]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_028583425.1/qc/individuals.samps.txt
[qc/qc_dashboard.html]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_028583425.1/qc/qc_dashboard.html
[qc/qc_report.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_028583425.1/qc/qc_report.tsv
[vcfs/filtered.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_028583425.1/vcfs/filtered.vcf.gz
[vcfs/filtered.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_028583425.1/vcfs/filtered.vcf.gz.tbi
[vcfs/raw.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_028583425.1/vcfs/raw.vcf.gz
[vcfs/raw.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_028583425.1/vcfs/raw.vcf.gz.tbi
<!-- congen:end body -->
