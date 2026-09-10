<!-- congen:begin body -->
# *Tautogolabrus adspersus*

Population-genomic variant calls for 150 *Tautogolabrus adspersus* samples ([NCBI taxon 34787](https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=34787)), produced by snpArcher against `GCA_020745685.1` and published on GenomeArk.

> [!TIP]
> **PASS** · validated 2026-09-10 · `GCA_020745685.1`
>
> Metadata agrees with the data published on GenomeArk.
>
> Full report: [`VALIDATION.md`](VALIDATION.md) · check definitions: [`CHECKS.md`](../../../CHECKS.md) · baseline description: [`README.txt`](README.txt)

## Dataset

|  |  |
|---|---|
| Clade | fishes |
| Reference assembly | [`GCA_020745685.1`](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_020745685.1/) — fTauAds1.pri.cur, Chromosome |
| Variant caller | gatk 4.6.2.0 |
| Samples | 150, all from SRA runs |
| Variant sites | 23,646,774 |

<details>
<summary>Assembly and pipeline detail</summary>

|  |  |
|---|---|
| Assembly organism | Tautogolabrus adspersus |
| NCBI taxon | 34787 |
| Contigs in the VCF | 64 |
| Ploidy / heterozygosity prior | 2 / 0.005 |
| Other tools recorded in the VCF header | bcftools 1.23.1 |

</details>

## Sample QC

Full plots: [the snpArcher QC dashboard][qc/qc_dashboard.html]. Per-sample values are not reproduced here; they are in [`qc_report.tsv`][qc/qc_report.tsv], [`individuals.het`][qc/individuals.het] and [`individuals.imiss`][qc/individuals.imiss].

**Mean depth 11.3×** (IQR 9.1–14.3, range 1.0× `SAMN35437697` to 24.7× `SAMN35437787`), over mapped reads.

| mean depth | samples |  |
|---|---:|---|
| 0–5× | 19 | █████████ |
| 5–10× | 37 | ██████████████████ |
| 10–15× | 63 | ██████████████████████████████ |
| 15–20× | 21 | ██████████ |
| 20–30× | 10 | █████ |

Across the cohort:

|  |  |
|---|---|
| Cohort mean coverage | 9.9× |
| Reads mapped | median 99.2% (range 30.5–99.8%) |
| Duplicates | median 15.1% (range 9.2–21.6%) |
| Properly paired | median 97.3% (range 25.4–98.4%) |
| Missingness F_MISS | median 0.081 (range 0.055–0.932) |
| Inbreeding coefficient F | median 0.111 (range -0.246–0.557) |

The callable-sites mask was built over depths 4–20× ([`coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv]). That is a site-level threshold for the mask, not a per-sample cutoff: samples outside it are neither excluded nor flagged here.

## References

*Not generated yet.* This section will list the BioProjects that contributed reads to this dataset, with a citation for each. Until then, the contributing BioProjects are recorded in [`README.txt`](README.txt) — please cite them when you use this dataset.

## Getting the data

Everything below is under `s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_020745685.1/`. The bucket is public and needs no credentials, but the AWS CLI needs `--no-sign-request` or it will try to sign the request and fail.

|  | Size |  |
|---|---:|---|
| [`raw.vcf.gz`][vcfs/raw.vcf.gz] | 15.9 GiB | unfiltered joint-genotyped calls |
| [`raw.vcf.gz.tbi`][vcfs/raw.vcf.gz.tbi] | 662.0 KiB | index for the above |
| [`callable_sites.bed`][callable_sites/callable_sites.bed] | 2.4 MiB | the callable-sites mask |
| [`qc_dashboard.html`][qc/qc_dashboard.html] | 11.8 MiB | the snpArcher QC report — opens in a browser |
| `bams/` | 650.0 GiB | 300 objects — alignments and indexes |

This run did not produce `filtered.vcf.gz`.

```bash
# one region of the VCF, without downloading the whole thing
bcftools view -r <chr>:<start>-<end> \
  https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_020745685.1/vcfs/raw.vcf.gz

# the alignments — check the size above first
aws s3 sync --no-sign-request \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_020745685.1/bams/ ./bams/

# the masks and QC tables, without the zarr stores
aws s3 sync --no-sign-request --exclude '*.zarr/*' \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_020745685.1/callable_sites/ ./callable_sites/
```

> [!WARNING]
> `callable_sites/` also holds `callable_loci.zarr/`, `depths/`, `depths.zarr/` and `genmap_index/` — zarr stores of thousands of small objects each.
>
> Nothing here lists or sizes them, so the sizes above are a lower bound, and a recursive `sync` without the `--exclude` above will pull all of them.

<details>
<summary>The other 10 published files</summary>

|  | Size |
|---|---:|
| [`qc/qc_report.tsv`][qc/qc_report.tsv] | 13.3 KiB |
| [`qc/individuals.idepth`][qc/individuals.idepth] | 4.4 KiB |
| [`qc/individuals.het`][qc/individuals.het] | 7.3 KiB |
| [`qc/individuals.imiss`][qc/individuals.imiss] | 6.2 KiB |
| [`qc/individuals.samps.txt`][qc/individuals.samps.txt] | 1.9 KiB |
| [`qc/contig_map.tsv`][qc/contig_map.tsv] | 2.1 KiB |
| [`callable_sites/coverage.bed`][callable_sites/coverage.bed] | 0 B |
| [`callable_sites/mappability.bed`][callable_sites/mappability.bed] | 2.4 MiB |
| [`callable_sites/mappability.bedgraph`][callable_sites/mappability.bedgraph] | 328.3 MiB |
| [`callable_sites/coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv] | 61 B |

</details>

[callable_sites/callable_sites.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_020745685.1/callable_sites/callable_sites.bed
[callable_sites/coverage.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_020745685.1/callable_sites/coverage.bed
[callable_sites/coverage_thresholds.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_020745685.1/callable_sites/coverage_thresholds.tsv
[callable_sites/mappability.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_020745685.1/callable_sites/mappability.bed
[callable_sites/mappability.bedgraph]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_020745685.1/callable_sites/mappability.bedgraph
[qc/contig_map.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_020745685.1/qc/contig_map.tsv
[qc/individuals.het]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_020745685.1/qc/individuals.het
[qc/individuals.idepth]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_020745685.1/qc/individuals.idepth
[qc/individuals.imiss]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_020745685.1/qc/individuals.imiss
[qc/individuals.samps.txt]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_020745685.1/qc/individuals.samps.txt
[qc/qc_dashboard.html]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_020745685.1/qc/qc_dashboard.html
[qc/qc_report.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_020745685.1/qc/qc_report.tsv
[vcfs/raw.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_020745685.1/vcfs/raw.vcf.gz
[vcfs/raw.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_020745685.1/vcfs/raw.vcf.gz.tbi
<!-- congen:end body -->
