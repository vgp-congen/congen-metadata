<!-- congen:begin body -->
# *Sturnus vulgaris*

Population-genomic variant calls for 78 *Sturnus vulgaris* samples ([NCBI taxon 9172](https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=9172)), produced by snpArcher against `GCA_052056855.1` and published on GenomeArk.

> [!NOTE]
> **PASS WITH WARNINGS** · validated 2026-09-11 · `GCA_052056855.1`
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
| Reference assembly | [`GCA_052056855.1`](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_052056855.1/) — bStuVul1.hap1, Chromosome |
| Variant caller | gatk 4.6.2.0 |
| Samples | 78 across 86 sheet rows, all from SRA runs |
| Variant sites | 41,268,775 |

<details>
<summary>Assembly and pipeline detail</summary>

|  |  |
|---|---|
| Assembly organism | Sturnus vulgaris |
| Paired RefSeq/GenBank accession | `GCF_052056855.1` |
| NCBI taxon | 9172 |
| Contigs in the VCF | 1011 |
| Ploidy / heterozygosity prior | 2 / 0.005 |
| Other tools recorded in the VCF header | bcftools 1.23.1 |

</details>

## Sample QC

Full plots: [the snpArcher QC dashboard][qc/qc_dashboard.html]. Per-sample values are not reproduced here; they are in [`qc_report.tsv`][qc/qc_report.tsv], [`individuals.het`][qc/individuals.het] and [`individuals.imiss`][qc/individuals.imiss].

**Mean depth 14.3×** (IQR 0.2–22.1, range 0.0× `SAMN04197021` to 69.1× `SAMN04029017`), over mapped reads.

| mean depth | samples |  |
|---|---:|---|
| 0–5× | 21 | █████████████████████████████ |
| 5–10× | 0 |  |
| 10–15× | 22 | ██████████████████████████████ |
| 15–20× | 10 | ██████████████ |
| 20–30× | 17 | ███████████████████████ |
| 30–40× | 3 | ████ |
| 40–60× | 3 | ████ |
| 60–100× | 2 | ███ |

Across the cohort:

|  |  |
|---|---|
| Cohort mean coverage | 15.5× |
| Reads mapped | median 99.5% (range 82.0–99.8%) |
| Duplicates | median 11.7% (range 0.2–17.0%) |
| Properly paired | median 97.8% (range 79.9–98.7%) |
| Missingness F_MISS | median 0.106 (range 0.070–0.993) |
| Inbreeding coefficient F | median 0.081 (range -0.053–0.996) |

The callable-sites mask was built over depths 7–32× ([`coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv]). That is a site-level threshold for the mask, not a per-sample cutoff: samples outside it are neither excluded nor flagged here.

## References

> [!CAUTION]
> **This dataset cannot yet be cited.**
>
> The recorded list of contributing BioProjects is known to be incorrect, so it is not reproduced here — a wrong citation list is worse than none, because it propagates into other people's papers. Do not publish analyses of this dataset until it is corrected.
>
> Reported by `E002` and `E003` — see [`VALIDATION.md`](VALIDATION.md) for what these mean and how to fix it.

## Getting the data

Everything below is under `s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_052056855.1/`. The bucket is public and needs no credentials, but the AWS CLI needs `--no-sign-request` or it will try to sign the request and fail.

|  | Size |  |
|---|---:|---|
| [`raw.vcf.gz`][vcfs/raw.vcf.gz] | 15.7 GiB | unfiltered joint-genotyped calls |
| [`raw.vcf.gz.tbi`][vcfs/raw.vcf.gz.tbi] | 1.1 MiB | index for the above |
| [`filtered.vcf.gz`][vcfs/filtered.vcf.gz] | 21.5 GiB | filtered calls |
| [`filtered.vcf.gz.tbi`][vcfs/filtered.vcf.gz.tbi] | 1.1 MiB | index for the above |
| [`callable_sites.bed`][callable_sites/callable_sites.bed] | 5.2 MiB | the callable-sites mask |
| [`qc_dashboard.html`][qc/qc_dashboard.html] | 9.3 MiB | the snpArcher QC report — opens in a browser |
| `bams/` | 806.2 GiB | 156 objects — alignments and indexes |

```bash
# one region of the VCF, without downloading the whole thing
bcftools view -r <chr>:<start>-<end> \
  https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_052056855.1/vcfs/raw.vcf.gz

# the alignments — check the size above first
aws s3 sync --no-sign-request \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_052056855.1/bams/ ./bams/

# the masks and QC tables, without the zarr stores
aws s3 sync --no-sign-request --exclude '*.zarr/*' \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_052056855.1/callable_sites/ ./callable_sites/
```

> [!WARNING]
> `callable_sites/` also holds `callable_loci.zarr/`, `depths/`, `depths.zarr/` and `genmap_index/` — zarr stores of thousands of small objects each.
>
> Nothing here lists or sizes them, so the sizes above are a lower bound, and a recursive `sync` without the `--exclude` above will pull all of them.

<details>
<summary>The other 10 published files</summary>

|  | Size |
|---|---:|
| [`qc/qc_report.tsv`][qc/qc_report.tsv] | 7.0 KiB |
| [`qc/individuals.idepth`][qc/individuals.idepth] | 2.3 KiB |
| [`qc/individuals.het`][qc/individuals.het] | 3.8 KiB |
| [`qc/individuals.imiss`][qc/individuals.imiss] | 3.2 KiB |
| [`qc/individuals.samps.txt`][qc/individuals.samps.txt] | 1014 B |
| [`qc/contig_map.tsv`][qc/contig_map.tsv] | 38.9 KiB |
| [`callable_sites/coverage.bed`][callable_sites/coverage.bed] | 0 B |
| [`callable_sites/mappability.bed`][callable_sites/mappability.bed] | 5.2 MiB |
| [`callable_sites/mappability.bedgraph`][callable_sites/mappability.bedgraph] | 3.7 GiB |
| [`callable_sites/coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv] | 62 B |

</details>

[callable_sites/callable_sites.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_052056855.1/callable_sites/callable_sites.bed
[callable_sites/coverage.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_052056855.1/callable_sites/coverage.bed
[callable_sites/coverage_thresholds.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_052056855.1/callable_sites/coverage_thresholds.tsv
[callable_sites/mappability.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_052056855.1/callable_sites/mappability.bed
[callable_sites/mappability.bedgraph]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_052056855.1/callable_sites/mappability.bedgraph
[qc/contig_map.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_052056855.1/qc/contig_map.tsv
[qc/individuals.het]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_052056855.1/qc/individuals.het
[qc/individuals.idepth]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_052056855.1/qc/individuals.idepth
[qc/individuals.imiss]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_052056855.1/qc/individuals.imiss
[qc/individuals.samps.txt]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_052056855.1/qc/individuals.samps.txt
[qc/qc_dashboard.html]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_052056855.1/qc/qc_dashboard.html
[qc/qc_report.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_052056855.1/qc/qc_report.tsv
[vcfs/filtered.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_052056855.1/vcfs/filtered.vcf.gz
[vcfs/filtered.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_052056855.1/vcfs/filtered.vcf.gz.tbi
[vcfs/raw.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_052056855.1/vcfs/raw.vcf.gz
[vcfs/raw.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_052056855.1/vcfs/raw.vcf.gz.tbi
<!-- congen:end body -->
