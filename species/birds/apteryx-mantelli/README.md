<!-- congen:begin body -->
# *Apteryx mantelli*

Population-genomic variant calls for 19 *Apteryx mantelli* samples ([NCBI taxon 2696672](https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=2696672)), produced by snpArcher against `GCA_036417845.1` and published on GenomeArk.

> [!TIP]
> **PASS** · validated 2026-09-10 · `GCA_036417845.1`
>
> Metadata agrees with the data published on GenomeArk.
>
> Full report: [`VALIDATION.md`](VALIDATION.md) · check definitions: [`CHECKS.md`](../../../CHECKS.md) · baseline description: [`README.txt`](README.txt)

## Dataset

|  |  |
|---|---|
| Clade | birds |
| Reference assembly | [`GCA_036417845.1`](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_036417845.1/) — bAptMan1.hap1, Chromosome |
| Variant caller | gatk 4.6.2.0 |
| Samples | 19, all from SRA runs |
| Variant sites | 31,253,369 |

<details>
<summary>Assembly and pipeline detail</summary>

|  |  |
|---|---|
| Assembly organism | Apteryx mantelli |
| Paired RefSeq/GenBank accession | `GCF_036417845.1` |
| NCBI taxon | 2696672 |
| Contigs in the VCF | 409 |
| Ploidy / heterozygosity prior | 2 / 0.005 |
| Other tools recorded in the VCF header | bcftools 1.23.1 |

</details>

## Sample QC

Full plots: [the snpArcher QC dashboard][qc/qc_dashboard.html]. Per-sample values are not reproduced here; they are in [`qc_report.tsv`][qc/qc_report.tsv], [`individuals.het`][qc/individuals.het] and [`individuals.imiss`][qc/individuals.imiss].

**Mean depth 10.5×** (IQR 9.5–12.2, range 8.4× `SAMN20169426` to 18.4× `SAMEA2554516`), over mapped reads.

| mean depth | samples |  |
|---|---:|---|
| 5–10× | 6 | ███████████████ |
| 10–15× | 12 | ██████████████████████████████ |
| 15–20× | 1 | ██ |

Across the cohort:

|  |  |
|---|---|
| Cohort mean coverage | 10.9× |
| Reads mapped | median 99.6% (range 87.8–99.8%) |
| Duplicates | median 17.9% (range 14.1–49.1%) |
| Properly paired | median 97.5% (range 84.3–98.4%) |
| Missingness F_MISS | median 0.061 (range 0.050–0.130) |
| Inbreeding coefficient F | median 0.117 (range -0.796–0.405) |

The callable-sites mask was built over depths 5–22× ([`coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv]). That is a site-level threshold for the mask, not a per-sample cutoff: samples outside it are neither excluded nor flagged here.

## References

*Not generated yet.* This section will list the BioProjects that contributed reads to this dataset, with a citation for each. Until then, the contributing BioProjects are recorded in [`README.txt`](README.txt) — please cite them when you use this dataset.

## Getting the data

Everything below is under `s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_036417845.1/`. The bucket is public and needs no credentials, but the AWS CLI needs `--no-sign-request` or it will try to sign the request and fail.

|  | Size |  |
|---|---:|---|
| [`raw.vcf.gz`][vcfs/raw.vcf.gz] | 4.0 GiB | unfiltered joint-genotyped calls |
| [`raw.vcf.gz.tbi`][vcfs/raw.vcf.gz.tbi] | 1.2 MiB | index for the above |
| [`callable_sites.bed`][callable_sites/callable_sites.bed] | 9.4 MiB | the callable-sites mask |
| [`qc_dashboard.html`][qc/qc_dashboard.html] | 8.6 MiB | the snpArcher QC report — opens in a browser |
| `bams/` | 193.4 GiB | 38 objects — alignments and indexes |

This run did not produce `filtered.vcf.gz`.

```bash
# one region of the VCF, without downloading the whole thing
bcftools view -r <chr>:<start>-<end> \
  https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_036417845.1/vcfs/raw.vcf.gz

# the alignments — check the size above first
aws s3 sync --no-sign-request \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_036417845.1/bams/ ./bams/

# the masks and QC tables, without the zarr stores
aws s3 sync --no-sign-request --exclude '*.zarr/*' \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_036417845.1/callable_sites/ ./callable_sites/
```

> [!WARNING]
> `callable_sites/` also holds `callable_loci.zarr/`, `depths/`, `depths.zarr/` and `genmap_index/` — zarr stores of thousands of small objects each.
>
> Nothing here lists or sizes them, so the sizes above are a lower bound, and a recursive `sync` without the `--exclude` above will pull all of them.

<details>
<summary>The other 10 published files</summary>

|  | Size |
|---|---:|
| [`qc/qc_report.tsv`][qc/qc_report.tsv] | 1.9 KiB |
| [`qc/individuals.idepth`][qc/individuals.idepth] | 592 B |
| [`qc/individuals.het`][qc/individuals.het] | 984 B |
| [`qc/individuals.imiss`][qc/individuals.imiss] | 839 B |
| [`qc/individuals.samps.txt`][qc/individuals.samps.txt] | 247 B |
| [`qc/contig_map.tsv`][qc/contig_map.tsv] | 15.3 KiB |
| [`callable_sites/coverage.bed`][callable_sites/coverage.bed] | 33.4 MiB |
| [`callable_sites/mappability.bed`][callable_sites/mappability.bed] | 4.8 MiB |
| [`callable_sites/mappability.bedgraph`][callable_sites/mappability.bedgraph] | 1.9 GiB |
| [`callable_sites/coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv] | 62 B |

</details>

[callable_sites/callable_sites.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_036417845.1/callable_sites/callable_sites.bed
[callable_sites/coverage.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_036417845.1/callable_sites/coverage.bed
[callable_sites/coverage_thresholds.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_036417845.1/callable_sites/coverage_thresholds.tsv
[callable_sites/mappability.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_036417845.1/callable_sites/mappability.bed
[callable_sites/mappability.bedgraph]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_036417845.1/callable_sites/mappability.bedgraph
[qc/contig_map.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_036417845.1/qc/contig_map.tsv
[qc/individuals.het]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_036417845.1/qc/individuals.het
[qc/individuals.idepth]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_036417845.1/qc/individuals.idepth
[qc/individuals.imiss]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_036417845.1/qc/individuals.imiss
[qc/individuals.samps.txt]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_036417845.1/qc/individuals.samps.txt
[qc/qc_dashboard.html]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_036417845.1/qc/qc_dashboard.html
[qc/qc_report.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_036417845.1/qc/qc_report.tsv
[vcfs/raw.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_036417845.1/vcfs/raw.vcf.gz
[vcfs/raw.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_036417845.1/vcfs/raw.vcf.gz.tbi
<!-- congen:end body -->
