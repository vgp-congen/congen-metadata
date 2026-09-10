<!-- congen:begin body -->
# *Lycaon pictus*

Population-genomic variant calls for 28 *Lycaon pictus* samples ([NCBI taxon 9622](https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=9622)), produced by snpArcher against `GCA_040955705.1` and published on GenomeArk.

> [!TIP]
> **PASS** · validated 2026-09-10 · `GCA_040955705.1`
>
> Metadata agrees with the data published on GenomeArk.
>
> Full report: [`VALIDATION.md`](VALIDATION.md) · check definitions: [`CHECKS.md`](../../../CHECKS.md) · baseline description: [`README.txt`](README.txt)

## Dataset

|  |  |
|---|---|
| Clade | mammals |
| Reference assembly | [`GCA_040955705.1`](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_040955705.1/) — LycPic1.final.hap2, Chromosome |
| Variant caller | gatk 4.6.2.0 |
| Samples | 28, all from SRA runs |
| Variant sites | 18,879,346 |

<details>
<summary>Assembly and pipeline detail</summary>

|  |  |
|---|---|
| Assembly organism | Lycaon pictus |
| NCBI taxon | 9622 |
| Contigs in the VCF | 76 |
| Ploidy / heterozygosity prior | 2 / 0.005 |
| Other tools recorded in the VCF header | bcftools 1.23.1 |

</details>

## Sample QC

Full plots: [the snpArcher QC dashboard][qc/qc_dashboard.html]. Per-sample values are not reproduced here; they are in [`qc_report.tsv`][qc/qc_report.tsv], [`individuals.het`][qc/individuals.het] and [`individuals.imiss`][qc/individuals.imiss].

**Mean depth 7.6×** (IQR 7.1–9.4, range 6.1× `SAMN50885746` to 48.6× `SAMN09917439`), over mapped reads.

| mean depth | samples |  |
|---|---:|---|
| 5–10× | 21 | ██████████████████████████████ |
| 10–15× | 2 | ███ |
| 15–20× | 1 | █ |
| 20–30× | 2 | ███ |
| 30–40× | 1 | █ |
| 40–60× | 1 | █ |

Across the cohort:

|  |  |
|---|---|
| Cohort mean coverage | 10.7× |
| Reads mapped | median 99.6% (range 85.7–100.0%) |
| Duplicates | median 42.5% (range 1.6–51.4%) |
| Properly paired | median 97.8% (range 84.5–99.5%) |
| Missingness F_MISS | median 0.083 (range 0.026–0.237) |
| Inbreeding coefficient F | median 0.002 (range -0.363–0.549) |

The callable-sites mask was built over depths 5–22× ([`coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv]). That is a site-level threshold for the mask, not a per-sample cutoff: samples outside it are neither excluded nor flagged here.

## References

*Not generated yet.* This section will list the BioProjects that contributed reads to this dataset, with a citation for each. Until then, the contributing BioProjects are recorded in [`README.txt`](README.txt) — please cite them when you use this dataset.

## Getting the data

Everything below is under `s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_040955705.1/`. The bucket is public and needs no credentials, but the AWS CLI needs `--no-sign-request` or it will try to sign the request and fail.

|  | Size |  |
|---|---:|---|
| [`raw.vcf.gz`][vcfs/raw.vcf.gz] | 3.7 GiB | unfiltered joint-genotyped calls |
| [`raw.vcf.gz.tbi`][vcfs/raw.vcf.gz.tbi] | 2.0 MiB | index for the above |
| [`callable_sites.bed`][callable_sites/callable_sites.bed] | 3.6 MiB | the callable-sites mask |
| [`qc_dashboard.html`][qc/qc_dashboard.html] | 8.8 MiB | the snpArcher QC report — opens in a browser |
| `bams/` | 485.1 GiB | 56 objects — alignments and indexes |

This run did not produce `filtered.vcf.gz`.

```bash
# one region of the VCF, without downloading the whole thing
bcftools view -r <chr>:<start>-<end> \
  https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_040955705.1/vcfs/raw.vcf.gz

# the alignments — check the size above first
aws s3 sync --no-sign-request \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_040955705.1/bams/ ./bams/

# the masks and QC tables, without the zarr stores
aws s3 sync --no-sign-request --exclude '*.zarr/*' \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_040955705.1/callable_sites/ ./callable_sites/
```

> [!WARNING]
> `callable_sites/` also holds `callable_loci.zarr/`, `depths/`, `depths.zarr/` and `genmap_index/` — zarr stores of thousands of small objects each.
>
> Nothing here lists or sizes them, so the sizes above are a lower bound, and a recursive `sync` without the `--exclude` above will pull all of them.

<details>
<summary>The other 10 published files</summary>

|  | Size |
|---|---:|
| [`qc/qc_report.tsv`][qc/qc_report.tsv] | 2.7 KiB |
| [`qc/individuals.idepth`][qc/individuals.idepth] | 859 B |
| [`qc/individuals.het`][qc/individuals.het] | 1.4 KiB |
| [`qc/individuals.imiss`][qc/individuals.imiss] | 1.2 KiB |
| [`qc/individuals.samps.txt`][qc/individuals.samps.txt] | 364 B |
| [`qc/contig_map.tsv`][qc/contig_map.tsv] | 2.4 KiB |
| [`callable_sites/coverage.bed`][callable_sites/coverage.bed] | 30.8 MiB |
| [`callable_sites/mappability.bed`][callable_sites/mappability.bed] | 2.8 MiB |
| [`callable_sites/mappability.bedgraph`][callable_sites/mappability.bedgraph] | 1.2 GiB |
| [`callable_sites/coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv] | 62 B |

</details>

[callable_sites/callable_sites.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_040955705.1/callable_sites/callable_sites.bed
[callable_sites/coverage.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_040955705.1/callable_sites/coverage.bed
[callable_sites/coverage_thresholds.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_040955705.1/callable_sites/coverage_thresholds.tsv
[callable_sites/mappability.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_040955705.1/callable_sites/mappability.bed
[callable_sites/mappability.bedgraph]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_040955705.1/callable_sites/mappability.bedgraph
[qc/contig_map.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_040955705.1/qc/contig_map.tsv
[qc/individuals.het]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_040955705.1/qc/individuals.het
[qc/individuals.idepth]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_040955705.1/qc/individuals.idepth
[qc/individuals.imiss]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_040955705.1/qc/individuals.imiss
[qc/individuals.samps.txt]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_040955705.1/qc/individuals.samps.txt
[qc/qc_dashboard.html]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_040955705.1/qc/qc_dashboard.html
[qc/qc_report.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_040955705.1/qc/qc_report.tsv
[vcfs/raw.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_040955705.1/vcfs/raw.vcf.gz
[vcfs/raw.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_040955705.1/vcfs/raw.vcf.gz.tbi
<!-- congen:end body -->
