<!-- congen:begin body -->
# *Scophthalmus maximus*

Population-genomic variant calls for 15 *Scophthalmus maximus* samples ([NCBI taxon 52904](https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=52904)), produced by snpArcher against `GCA_963854745.1` and published on GenomeArk.

> [!TIP]
> **PASS** · validated 2026-09-10 · `GCA_963854745.1`
>
> Metadata agrees with the data published on GenomeArk.
>
> Full report: [`VALIDATION.md`](VALIDATION.md) · check definitions: [`CHECKS.md`](../../../CHECKS.md) · baseline description: [`README.txt`](README.txt)

## Dataset

|  |  |
|---|---|
| Clade | fishes |
| Reference assembly | [`GCA_963854745.1`](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_963854745.1/) — fScoMax1.1, Chromosome |
| Variant caller | gatk 4.6.2.0 |
| Samples | 15 across 23 sheet rows, all from SRA runs |
| Variant sites | 6,239,765 |

<details>
<summary>Assembly and pipeline detail</summary>

|  |  |
|---|---|
| Assembly organism | Scophthalmus maximus |
| NCBI taxon | 52904 |
| Contigs in the VCF | 108 |
| Ploidy / heterozygosity prior | 2 / 0.005 |
| Other tools recorded in the VCF header | bcftools 1.23.1 |

</details>

## Sample QC

Full plots: [the snpArcher QC dashboard][qc/qc_dashboard.html]. Per-sample values are not reproduced here; they are in [`qc_report.tsv`][qc/qc_report.tsv], [`individuals.het`][qc/individuals.het] and [`individuals.imiss`][qc/individuals.imiss].

**Mean depth 15.4×** (IQR 13.8–17.2, range 12.2× `SAMN15907225` to 139.8× `SAMN15667284`), over mapped reads.

| mean depth | samples |  |
|---|---:|---|
| 10–15× | 7 | ██████████████████████████████ |
| 15–20× | 5 | █████████████████████ |
| 20–30× | 0 |  |
| 30–40× | 1 | ████ |
| 40–60× | 0 |  |
| 60–100× | 1 | ████ |
| 100×+ | 1 | ████ |

Across the cohort:

|  |  |
|---|---|
| Cohort mean coverage | 28.0× |
| Reads mapped | median 99.4% (range 95.0–99.4%) |
| Duplicates | median 10.1% (range 1.6–14.6%) |
| Properly paired | median 95.1% (range 84.6–97.0%) |
| Missingness F_MISS | median 0.063 (range 0.037–0.192) |
| Inbreeding coefficient F | median 0.009 (range -1.258–0.927) |

The callable-sites mask was built over depths 13–56× ([`coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv]). That is a site-level threshold for the mask, not a per-sample cutoff: samples outside it are neither excluded nor flagged here.

## References

*Not generated yet.* This section will list the BioProjects that contributed reads to this dataset, with a citation for each. Until then, the contributing BioProjects are recorded in [`README.txt`](README.txt) — please cite them when you use this dataset.

## Getting the data

Everything below is under `s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_963854745.1/`. The bucket is public and needs no credentials, but the AWS CLI needs `--no-sign-request` or it will try to sign the request and fail.

|  | Size |  |
|---|---:|---|
| [`raw.vcf.gz`][vcfs/raw.vcf.gz] | 983.7 MiB | unfiltered joint-genotyped calls |
| [`raw.vcf.gz.tbi`][vcfs/raw.vcf.gz.tbi] | 402.2 KiB | index for the above |
| [`callable_sites.bed`][callable_sites/callable_sites.bed] | 752.9 KiB | the callable-sites mask |
| [`qc_dashboard.html`][qc/qc_dashboard.html] | 8.5 MiB | the snpArcher QC report — opens in a browser |
| `bams/` | 121.3 GiB | 30 objects — alignments and indexes |

This run did not produce `filtered.vcf.gz`.

```bash
# one region of the VCF, without downloading the whole thing
bcftools view -r <chr>:<start>-<end> \
  https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_963854745.1/vcfs/raw.vcf.gz

# the alignments — check the size above first
aws s3 sync --no-sign-request \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_963854745.1/bams/ ./bams/

# the masks and QC tables, without the zarr stores
aws s3 sync --no-sign-request --exclude '*.zarr/*' \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_963854745.1/callable_sites/ ./callable_sites/
```

> [!WARNING]
> `callable_sites/` also holds `callable_loci.zarr/`, `depths/`, `depths.zarr/` and `genmap_index/` — zarr stores of thousands of small objects each.
>
> Nothing here lists or sizes them, so the sizes above are a lower bound, and a recursive `sync` without the `--exclude` above will pull all of them.

<details>
<summary>The other 10 published files</summary>

|  | Size |
|---|---:|
| [`qc/qc_report.tsv`][qc/qc_report.tsv] | 1.5 KiB |
| [`qc/individuals.idepth`][qc/individuals.idepth] | 456 B |
| [`qc/individuals.het`][qc/individuals.het] | 739 B |
| [`qc/individuals.imiss`][qc/individuals.imiss] | 644 B |
| [`qc/individuals.samps.txt`][qc/individuals.samps.txt] | 195 B |
| [`qc/contig_map.tsv`][qc/contig_map.tsv] | 3.8 KiB |
| [`callable_sites/coverage.bed`][callable_sites/coverage.bed] | 674.5 KiB |
| [`callable_sites/mappability.bed`][callable_sites/mappability.bed] | 656.2 KiB |
| [`callable_sites/mappability.bedgraph`][callable_sites/mappability.bedgraph] | 138.2 MiB |
| [`callable_sites/coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv] | 63 B |

</details>

[callable_sites/callable_sites.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_963854745.1/callable_sites/callable_sites.bed
[callable_sites/coverage.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_963854745.1/callable_sites/coverage.bed
[callable_sites/coverage_thresholds.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_963854745.1/callable_sites/coverage_thresholds.tsv
[callable_sites/mappability.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_963854745.1/callable_sites/mappability.bed
[callable_sites/mappability.bedgraph]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_963854745.1/callable_sites/mappability.bedgraph
[qc/contig_map.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_963854745.1/qc/contig_map.tsv
[qc/individuals.het]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_963854745.1/qc/individuals.het
[qc/individuals.idepth]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_963854745.1/qc/individuals.idepth
[qc/individuals.imiss]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_963854745.1/qc/individuals.imiss
[qc/individuals.samps.txt]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_963854745.1/qc/individuals.samps.txt
[qc/qc_dashboard.html]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_963854745.1/qc/qc_dashboard.html
[qc/qc_report.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_963854745.1/qc/qc_report.tsv
[vcfs/raw.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_963854745.1/vcfs/raw.vcf.gz
[vcfs/raw.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_963854745.1/vcfs/raw.vcf.gz.tbi
<!-- congen:end body -->
