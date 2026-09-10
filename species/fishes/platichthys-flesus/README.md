<!-- congen:begin body -->
# *Platichthys flesus*

Population-genomic variant calls for 53 *Platichthys flesus* samples ([NCBI taxon 8260](https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=8260)), produced by snpArcher against `GCA_949316205.1` and published on GenomeArk.

> [!TIP]
> **PASS** · validated 2026-09-10 · `GCA_949316205.1`
>
> Metadata agrees with the data published on GenomeArk.
>
> Full report: [`VALIDATION.md`](VALIDATION.md) · check definitions: [`CHECKS.md`](../../../CHECKS.md) · baseline description: [`README.txt`](README.txt)

## Dataset

|  |  |
|---|---|
| Clade | fishes |
| Reference assembly | [`GCA_949316205.1`](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_949316205.1/) — fPlaFle2.1, Chromosome |
| Variant caller | gatk 4.6.2.0 |
| Samples | 53 across 64 sheet rows, all from SRA runs |
| Variant sites | 26,083,442 |

<details>
<summary>Assembly and pipeline detail</summary>

|  |  |
|---|---|
| Assembly organism | Platichthys flesus |
| Paired RefSeq/GenBank accession | `GCF_949316205.1` |
| NCBI taxon | 8260 |
| Contigs in the VCF | 109 |
| Ploidy / heterozygosity prior | 2 / 0.005 |
| Other tools recorded in the VCF header | bcftools 1.23.1 |

</details>

## Sample QC

Full plots: [the snpArcher QC dashboard][qc/qc_dashboard.html]. Per-sample values are not reproduced here; they are in [`qc_report.tsv`][qc/qc_report.tsv], [`individuals.het`][qc/individuals.het] and [`individuals.imiss`][qc/individuals.imiss].

**Mean depth 11.2×** (IQR 9.8–14.6, range 8.1× `SAMEA118444175` to 28.2× `SAMN31781931`), over mapped reads.

| mean depth | samples |  |
|---|---:|---|
| 5–10× | 16 | ███████████████████ |
| 10–15× | 25 | ██████████████████████████████ |
| 15–20× | 11 | █████████████ |
| 20–30× | 1 | █ |

Across the cohort:

|  |  |
|---|---|
| Cohort mean coverage | 9.6× |
| Reads mapped | median 99.7% (range 98.9–99.8%) |
| Duplicates | median 13.9% (range 10.2–26.4%) |
| Properly paired | median 98.1% (range 94.3–98.8%) |
| Missingness F_MISS | median 0.046 (range 0.027–0.071) |
| Inbreeding coefficient F | median 0.067 (range -0.026–0.251) |

The callable-sites mask was built over depths 4–20× ([`coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv]). That is a site-level threshold for the mask, not a per-sample cutoff: samples outside it are neither excluded nor flagged here.

## References

*Not generated yet.* This section will list the BioProjects that contributed reads to this dataset, with a citation for each. Until then, the contributing BioProjects are recorded in [`README.txt`](README.txt) — please cite them when you use this dataset.

## Getting the data

Everything below is under `s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_949316205.1/`. The bucket is public and needs no credentials, but the AWS CLI needs `--no-sign-request` or it will try to sign the request and fail.

|  | Size |  |
|---|---:|---|
| [`raw.vcf.gz`][vcfs/raw.vcf.gz] | 8.3 GiB | unfiltered joint-genotyped calls |
| [`raw.vcf.gz.tbi`][vcfs/raw.vcf.gz.tbi] | 513.4 KiB | index for the above |
| [`callable_sites.bed`][callable_sites/callable_sites.bed] | 1.8 MiB | the callable-sites mask |
| [`qc_dashboard.html`][qc/qc_dashboard.html] | 9.2 MiB | the snpArcher QC report — opens in a browser |
| `bams/` | 201.6 GiB | 106 objects — alignments and indexes |

This run did not produce `filtered.vcf.gz`.

```bash
# one region of the VCF, without downloading the whole thing
bcftools view -r <chr>:<start>-<end> \
  https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949316205.1/vcfs/raw.vcf.gz

# the alignments — check the size above first
aws s3 sync --no-sign-request \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_949316205.1/bams/ ./bams/

# the masks and QC tables, without the zarr stores
aws s3 sync --no-sign-request --exclude '*.zarr/*' \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_949316205.1/callable_sites/ ./callable_sites/
```

> [!WARNING]
> `callable_sites/` also holds `callable_loci.zarr/`, `depths/`, `depths.zarr/` and `genmap_index/` — zarr stores of thousands of small objects each.
>
> Nothing here lists or sizes them, so the sizes above are a lower bound, and a recursive `sync` without the `--exclude` above will pull all of them.

<details>
<summary>The other 10 published files</summary>

|  | Size |
|---|---:|
| [`qc/qc_report.tsv`][qc/qc_report.tsv] | 4.9 KiB |
| [`qc/individuals.idepth`][qc/individuals.idepth] | 1.7 KiB |
| [`qc/individuals.het`][qc/individuals.het] | 2.7 KiB |
| [`qc/individuals.imiss`][qc/individuals.imiss] | 2.3 KiB |
| [`qc/individuals.samps.txt`][qc/individuals.samps.txt] | 793 B |
| [`qc/contig_map.tsv`][qc/contig_map.tsv] | 3.9 KiB |
| [`callable_sites/coverage.bed`][callable_sites/coverage.bed] | 22.5 MiB |
| [`callable_sites/mappability.bed`][callable_sites/mappability.bed] | 1.4 MiB |
| [`callable_sites/mappability.bedgraph`][callable_sites/mappability.bedgraph] | 441.1 MiB |
| [`callable_sites/coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv] | 61 B |

</details>

[callable_sites/callable_sites.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949316205.1/callable_sites/callable_sites.bed
[callable_sites/coverage.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949316205.1/callable_sites/coverage.bed
[callable_sites/coverage_thresholds.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949316205.1/callable_sites/coverage_thresholds.tsv
[callable_sites/mappability.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949316205.1/callable_sites/mappability.bed
[callable_sites/mappability.bedgraph]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949316205.1/callable_sites/mappability.bedgraph
[qc/contig_map.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949316205.1/qc/contig_map.tsv
[qc/individuals.het]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949316205.1/qc/individuals.het
[qc/individuals.idepth]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949316205.1/qc/individuals.idepth
[qc/individuals.imiss]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949316205.1/qc/individuals.imiss
[qc/individuals.samps.txt]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949316205.1/qc/individuals.samps.txt
[qc/qc_dashboard.html]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949316205.1/qc/qc_dashboard.html
[qc/qc_report.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949316205.1/qc/qc_report.tsv
[vcfs/raw.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949316205.1/vcfs/raw.vcf.gz
[vcfs/raw.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949316205.1/vcfs/raw.vcf.gz.tbi
<!-- congen:end body -->
