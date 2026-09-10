<!-- congen:begin body -->
# *Shinisaurus crocodilurus*

Population-genomic variant calls for 11 *Shinisaurus crocodilurus* samples ([NCBI taxon 52224](https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=52224)), produced by snpArcher against `GCA_021292165.1` and published on GenomeArk.

> [!NOTE]
> **PASS WITH WARNINGS** · validated 2026-09-10 · `GCA_021292165.1`
>
> Metadata agrees with the data published on GenomeArk, with 2 warnings to note.
>
> Full report: [`VALIDATION.md`](VALIDATION.md) · check definitions: [`CHECKS.md`](../../../CHECKS.md) · baseline description: [`README.txt`](README.txt)

## Dataset

|  |  |
|---|---|
| Clade | reptiles |
| Reference assembly | [`GCA_021292165.1`](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_021292165.1/) — IOZ_Scro_1.0, Chromosome |
| Variant caller | gatk 4.6.2.0 |
| Samples | 11 across 12 sheet rows, from 1 local FASTQ, 11 SRA runs |
| Variant sites | 31,999,026 |

<details>
<summary>Assembly and pipeline detail</summary>

|  |  |
|---|---|
| Assembly organism | Shinisaurus crocodilurus |
| NCBI taxon | 52224 |
| Contigs in the VCF | 1553 |
| Ploidy / heterozygosity prior | 2 / 0.005 |
| Other tools recorded in the VCF header | bcftools 1.23.1 |

</details>

## Sample QC

Full plots: [the snpArcher QC dashboard][qc/qc_dashboard.html]. Per-sample values are not reproduced here; they are in [`qc_report.tsv`][qc/qc_report.tsv], [`individuals.het`][qc/individuals.het] and [`individuals.imiss`][qc/individuals.imiss].

**Mean depth 10.7×** (IQR 10.0–11.0, range 9.7× `SAMN19090611` to 30.5× `SAMN19072228`), over mapped reads.

| mean depth | samples |  |
|---|---:|---|
| 5–10× | 3 | ███████████████ |
| 10–15× | 6 | ██████████████████████████████ |
| 15–20× | 0 |  |
| 20–30× | 1 | █████ |
| 30–40× | 1 | █████ |

Across the cohort:

|  |  |
|---|---|
| Cohort mean coverage | 12.9× |
| Reads mapped | median 100.0% (range 98.3–100.0%) |
| Duplicates | median 0.4% (range 0.3–10.4%) |
| Properly paired | median 97.2% (range 88.5–97.4%) |
| Missingness F_MISS | median 0.075 (range 0.067–0.154) |
| Inbreeding coefficient F | median 0.413 (range -0.217–0.617) |

The callable-sites mask was built over depths 6–26× ([`coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv]). That is a site-level threshold for the mask, not a per-sample cutoff: samples outside it are neither excluded nor flagged here.

## References

*Not generated yet.* This section will list the BioProjects that contributed reads to this dataset, with a citation for each. Until then, the contributing BioProjects are recorded in [`README.txt`](README.txt) — please cite them when you use this dataset.

## Getting the data

Everything below is under `s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_021292165.1/`. The bucket is public and needs no credentials, but the AWS CLI needs `--no-sign-request` or it will try to sign the request and fail.

|  | Size |  |
|---|---:|---|
| [`raw.vcf.gz`][vcfs/raw.vcf.gz] | 3.4 GiB | unfiltered joint-genotyped calls |
| [`raw.vcf.gz.tbi`][vcfs/raw.vcf.gz.tbi] | 1.7 MiB | index for the above |
| [`callable_sites.bed`][callable_sites/callable_sites.bed] | 24.1 MiB | the callable-sites mask |
| [`qc_dashboard.html`][qc/qc_dashboard.html] | 8.4 MiB | the snpArcher QC report — opens in a browser |
| `bams/` | 164.4 GiB | 22 objects — alignments and indexes |

This run did not produce `filtered.vcf.gz`.

```bash
# one region of the VCF, without downloading the whole thing
bcftools view -r <chr>:<start>-<end> \
  https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_021292165.1/vcfs/raw.vcf.gz

# the alignments — check the size above first
aws s3 sync --no-sign-request \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_021292165.1/bams/ ./bams/

# the masks and QC tables, without the zarr stores
aws s3 sync --no-sign-request --exclude '*.zarr/*' \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_021292165.1/callable_sites/ ./callable_sites/
```

> [!WARNING]
> `callable_sites/` also holds `callable_loci.zarr/`, `depths/`, `depths.zarr/` and `genmap_index/` — zarr stores of thousands of small objects each.
>
> Nothing here lists or sizes them, so the sizes above are a lower bound, and a recursive `sync` without the `--exclude` above will pull all of them.

<details>
<summary>The other 10 published files</summary>

|  | Size |
|---|---:|
| [`qc/qc_report.tsv`][qc/qc_report.tsv] | 1.2 KiB |
| [`qc/individuals.idepth`][qc/individuals.idepth] | 353 B |
| [`qc/individuals.het`][qc/individuals.het] | 580 B |
| [`qc/individuals.imiss`][qc/individuals.imiss] | 505 B |
| [`qc/individuals.samps.txt`][qc/individuals.samps.txt] | 143 B |
| [`qc/contig_map.tsv`][qc/contig_map.tsv] | 60.9 KiB |
| [`callable_sites/coverage.bed`][callable_sites/coverage.bed] | 50.1 MiB |
| [`callable_sites/mappability.bed`][callable_sites/mappability.bed] | 22.2 MiB |
| [`callable_sites/mappability.bedgraph`][callable_sites/mappability.bedgraph] | 3.8 GiB |
| [`callable_sites/coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv] | 62 B |

</details>

[callable_sites/callable_sites.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_021292165.1/callable_sites/callable_sites.bed
[callable_sites/coverage.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_021292165.1/callable_sites/coverage.bed
[callable_sites/coverage_thresholds.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_021292165.1/callable_sites/coverage_thresholds.tsv
[callable_sites/mappability.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_021292165.1/callable_sites/mappability.bed
[callable_sites/mappability.bedgraph]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_021292165.1/callable_sites/mappability.bedgraph
[qc/contig_map.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_021292165.1/qc/contig_map.tsv
[qc/individuals.het]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_021292165.1/qc/individuals.het
[qc/individuals.idepth]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_021292165.1/qc/individuals.idepth
[qc/individuals.imiss]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_021292165.1/qc/individuals.imiss
[qc/individuals.samps.txt]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_021292165.1/qc/individuals.samps.txt
[qc/qc_dashboard.html]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_021292165.1/qc/qc_dashboard.html
[qc/qc_report.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_021292165.1/qc/qc_report.tsv
[vcfs/raw.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_021292165.1/vcfs/raw.vcf.gz
[vcfs/raw.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_021292165.1/vcfs/raw.vcf.gz.tbi
<!-- congen:end body -->
