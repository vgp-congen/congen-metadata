<!-- congen:begin body -->
# *Mastacembelus armatus*

Population-genomic variant calls for 28 *Mastacembelus armatus* samples ([NCBI taxon 205130](https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=205130)), produced by snpArcher against `GCA_900324485.3` and published on GenomeArk.

> [!TIP]
> **PASS** · validated 2026-09-10 · `GCA_900324485.3`
>
> Metadata agrees with the data published on GenomeArk.
>
> Full report: [`VALIDATION.md`](VALIDATION.md) · check definitions: [`CHECKS.md`](../../../CHECKS.md) · baseline description: [`README.txt`](README.txt)

## Dataset

|  |  |
|---|---|
| Clade | fishes |
| Reference assembly | [`GCA_900324485.3`](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_900324485.3/) — fMasArm1.3, Chromosome |
| Variant caller | gatk 4.6.2.0 |
| Samples | 28, all from SRA runs |
| Variant sites | 20,809,341 |

<details>
<summary>Assembly and pipeline detail</summary>

|  |  |
|---|---|
| Assembly organism | Mastacembelus armatus |
| Paired RefSeq/GenBank accession | `GCF_900324485.3` |
| NCBI taxon | 205130 |
| Contigs in the VCF | 123 |
| Ploidy / heterozygosity prior | 2 / 0.005 |
| Other tools recorded in the VCF header | bcftools 1.23.1 |

</details>

## Sample QC

Full plots: [the snpArcher QC dashboard][qc/qc_dashboard.html]. Per-sample values are not reproduced here; they are in [`qc_report.tsv`][qc/qc_report.tsv], [`individuals.het`][qc/individuals.het] and [`individuals.imiss`][qc/individuals.imiss].

**Mean depth 12.5×** (IQR 12.2–13.6, range 7.0× `SAMN31779410` to 57.5× `SAMN14858006`), over mapped reads.

| mean depth | samples |  |
|---|---:|---|
| 5–10× | 2 | ███ |
| 10–15× | 21 | ██████████████████████████████ |
| 15–20× | 1 | █ |
| 20–30× | 2 | ███ |
| 30–40× | 1 | █ |
| 40–60× | 1 | █ |

Across the cohort:

|  |  |
|---|---|
| Cohort mean coverage | 14.8× |
| Reads mapped | median 97.7% (range 96.8–98.2%) |
| Duplicates | median 18.9% (range 11.0–24.6%) |
| Properly paired | median 87.4% (range 33.9–92.0%) |
| Missingness F_MISS | median 0.103 (range 0.075–0.274) |
| Inbreeding coefficient F | median 0.483 (range 0.314–0.824) |

The callable-sites mask was built over depths 7–30× ([`coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv]). That is a site-level threshold for the mask, not a per-sample cutoff: samples outside it are neither excluded nor flagged here.

## References

*Not generated yet.* This section will list the BioProjects that contributed reads to this dataset, with a citation for each. Until then, the contributing BioProjects are recorded in [`README.txt`](README.txt) — please cite them when you use this dataset.

## Getting the data

Everything below is under `s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_900324485.3/`. The bucket is public and needs no credentials, but the AWS CLI needs `--no-sign-request` or it will try to sign the request and fail.

|  | Size |  |
|---|---:|---|
| [`raw.vcf.gz`][vcfs/raw.vcf.gz] | 3.7 GiB | unfiltered joint-genotyped calls |
| [`raw.vcf.gz.tbi`][vcfs/raw.vcf.gz.tbi] | 477.7 KiB | index for the above |
| [`callable_sites.bed`][callable_sites/callable_sites.bed] | 2.2 MiB | the callable-sites mask |
| [`qc_dashboard.html`][qc/qc_dashboard.html] | 8.8 MiB | the snpArcher QC report — opens in a browser |
| `bams/` | 151.4 GiB | 56 objects — alignments and indexes |

This run did not produce `filtered.vcf.gz`.

```bash
# one region of the VCF, without downloading the whole thing
bcftools view -r <chr>:<start>-<end> \
  https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_900324485.3/vcfs/raw.vcf.gz

# the alignments — check the size above first
aws s3 sync --no-sign-request \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_900324485.3/bams/ ./bams/

# the masks and QC tables, without the zarr stores
aws s3 sync --no-sign-request --exclude '*.zarr/*' \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_900324485.3/callable_sites/ ./callable_sites/
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
| [`qc/individuals.idepth`][qc/individuals.idepth] | 862 B |
| [`qc/individuals.het`][qc/individuals.het] | 1.4 KiB |
| [`qc/individuals.imiss`][qc/individuals.imiss] | 1.2 KiB |
| [`qc/individuals.samps.txt`][qc/individuals.samps.txt] | 366 B |
| [`qc/contig_map.tsv`][qc/contig_map.tsv] | 3.8 KiB |
| [`callable_sites/coverage.bed`][callable_sites/coverage.bed] | 7.0 MiB |
| [`callable_sites/mappability.bed`][callable_sites/mappability.bed] | 2.0 MiB |
| [`callable_sites/mappability.bedgraph`][callable_sites/mappability.bedgraph] | 405.5 MiB |
| [`callable_sites/coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv] | 62 B |

</details>

[callable_sites/callable_sites.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_900324485.3/callable_sites/callable_sites.bed
[callable_sites/coverage.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_900324485.3/callable_sites/coverage.bed
[callable_sites/coverage_thresholds.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_900324485.3/callable_sites/coverage_thresholds.tsv
[callable_sites/mappability.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_900324485.3/callable_sites/mappability.bed
[callable_sites/mappability.bedgraph]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_900324485.3/callable_sites/mappability.bedgraph
[qc/contig_map.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_900324485.3/qc/contig_map.tsv
[qc/individuals.het]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_900324485.3/qc/individuals.het
[qc/individuals.idepth]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_900324485.3/qc/individuals.idepth
[qc/individuals.imiss]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_900324485.3/qc/individuals.imiss
[qc/individuals.samps.txt]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_900324485.3/qc/individuals.samps.txt
[qc/qc_dashboard.html]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_900324485.3/qc/qc_dashboard.html
[qc/qc_report.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_900324485.3/qc/qc_report.tsv
[vcfs/raw.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_900324485.3/vcfs/raw.vcf.gz
[vcfs/raw.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_900324485.3/vcfs/raw.vcf.gz.tbi
<!-- congen:end body -->
