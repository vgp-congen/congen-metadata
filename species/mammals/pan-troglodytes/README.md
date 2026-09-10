<!-- congen:begin body -->
# *Pan troglodytes*

Population-genomic variant calls for 10 *Pan troglodytes* samples ([NCBI taxon 9598](https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=9598)), produced by snpArcher against `GCA_028858775.3` and published on GenomeArk.

> [!TIP]
> **PASS** · validated 2026-09-10 · `GCA_028858775.3`
>
> Metadata agrees with the data published on GenomeArk.
>
> Full report: [`VALIDATION.md`](VALIDATION.md) · check definitions: [`CHECKS.md`](../../../CHECKS.md) · baseline description: [`README.txt`](README.txt)

## Dataset

|  |  |
|---|---|
| Clade | mammals |
| Reference assembly | [`GCA_028858775.3`](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_028858775.3/) — NHGRI_mPanTro3-v2.1_pri, Chromosome |
| Variant caller | gatk 4.6.2.0 |
| Samples | 10, all from SRA runs |
| Variant sites | 21,304,966 |

<details>
<summary>Assembly and pipeline detail</summary>

|  |  |
|---|---|
| Assembly organism | Pan troglodytes |
| Paired RefSeq/GenBank accession | `GCF_028858775.2` |
| NCBI taxon | 9598 |
| Contigs in the VCF | 26 |
| Ploidy / heterozygosity prior | 2 / 0.005 |
| Other tools recorded in the VCF header | bcftools 1.23.1 |

</details>

## Sample QC

Full plots: [the snpArcher QC dashboard][qc/qc_dashboard.html]. Per-sample values are not reproduced here; they are in [`qc_report.tsv`][qc/qc_report.tsv], [`individuals.het`][qc/individuals.het] and [`individuals.imiss`][qc/individuals.imiss].

**Mean depth 28.2×** (IQR 27.0–36.9, range 15.7× `SAMN53084324` to 43.4× `SAMN53084319`), over mapped reads.

| mean depth | samples |  |
|---|---:|---|
| 15–20× | 2 | ████████████ |
| 20–30× | 5 | ██████████████████████████████ |
| 30–40× | 1 | ██████ |
| 40–60× | 2 | ████████████ |

Across the cohort:

|  |  |
|---|---|
| Cohort mean coverage | 29.3× |
| Reads mapped | median 100.0% (range 100.0–100.0%) |
| Duplicates | median 14.9% (range 7.5–27.1%) |
| Properly paired | median 98.6% (range 97.4–98.9%) |
| Missingness F_MISS | median 0.063 (range 0.054–0.076) |
| Inbreeding coefficient F | median -0.023 (range -0.422–0.380) |

The callable-sites mask was built over depths 14–59× ([`coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv]). That is a site-level threshold for the mask, not a per-sample cutoff: samples outside it are neither excluded nor flagged here.

## References

*Not generated yet.* This section will list the BioProjects that contributed reads to this dataset, with a citation for each. Until then, the contributing BioProjects are recorded in [`README.txt`](README.txt) — please cite them when you use this dataset.

## Getting the data

Everything below is under `s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_028858775.3/`. The bucket is public and needs no credentials, but the AWS CLI needs `--no-sign-request` or it will try to sign the request and fail.

|  | Size |  |
|---|---:|---|
| [`raw.vcf.gz`][vcfs/raw.vcf.gz] | 2.4 GiB | unfiltered joint-genotyped calls |
| [`raw.vcf.gz.tbi`][vcfs/raw.vcf.gz.tbi] | 2.3 MiB | index for the above |
| [`callable_sites.bed`][callable_sites/callable_sites.bed] | 13.9 MiB | the callable-sites mask |
| [`qc_dashboard.html`][qc/qc_dashboard.html] | 8.4 MiB | the snpArcher QC report — opens in a browser |
| `bams/` | 486.5 GiB | 20 objects — alignments and indexes |

This run did not produce `filtered.vcf.gz`.

```bash
# one region of the VCF, without downloading the whole thing
bcftools view -r <chr>:<start>-<end> \
  https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_028858775.3/vcfs/raw.vcf.gz

# the alignments — check the size above first
aws s3 sync --no-sign-request \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_028858775.3/bams/ ./bams/

# the masks and QC tables, without the zarr stores
aws s3 sync --no-sign-request --exclude '*.zarr/*' \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_028858775.3/callable_sites/ ./callable_sites/
```

> [!WARNING]
> `callable_sites/` also holds `callable_loci.zarr/`, `depths/`, `depths.zarr/` and `genmap_index/` — zarr stores of thousands of small objects each.
>
> Nothing here lists or sizes them, so the sizes above are a lower bound, and a recursive `sync` without the `--exclude` above will pull all of them.

<details>
<summary>The other 10 published files</summary>

|  | Size |
|---|---:|
| [`qc/qc_report.tsv`][qc/qc_report.tsv] | 1.1 KiB |
| [`qc/individuals.idepth`][qc/individuals.idepth] | 322 B |
| [`qc/individuals.het`][qc/individuals.het] | 534 B |
| [`qc/individuals.imiss`][qc/individuals.imiss] | 466 B |
| [`qc/individuals.samps.txt`][qc/individuals.samps.txt] | 130 B |
| [`qc/contig_map.tsv`][qc/contig_map.tsv] | 683 B |
| [`callable_sites/coverage.bed`][callable_sites/coverage.bed] | 19.5 MiB |
| [`callable_sites/mappability.bed`][callable_sites/mappability.bed] | 8.9 MiB |
| [`callable_sites/mappability.bedgraph`][callable_sites/mappability.bedgraph] | 3.8 GiB |
| [`callable_sites/coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv] | 63 B |

</details>

[callable_sites/callable_sites.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_028858775.3/callable_sites/callable_sites.bed
[callable_sites/coverage.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_028858775.3/callable_sites/coverage.bed
[callable_sites/coverage_thresholds.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_028858775.3/callable_sites/coverage_thresholds.tsv
[callable_sites/mappability.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_028858775.3/callable_sites/mappability.bed
[callable_sites/mappability.bedgraph]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_028858775.3/callable_sites/mappability.bedgraph
[qc/contig_map.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_028858775.3/qc/contig_map.tsv
[qc/individuals.het]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_028858775.3/qc/individuals.het
[qc/individuals.idepth]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_028858775.3/qc/individuals.idepth
[qc/individuals.imiss]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_028858775.3/qc/individuals.imiss
[qc/individuals.samps.txt]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_028858775.3/qc/individuals.samps.txt
[qc/qc_dashboard.html]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_028858775.3/qc/qc_dashboard.html
[qc/qc_report.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_028858775.3/qc/qc_report.tsv
[vcfs/raw.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_028858775.3/vcfs/raw.vcf.gz
[vcfs/raw.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_028858775.3/vcfs/raw.vcf.gz.tbi
<!-- congen:end body -->
