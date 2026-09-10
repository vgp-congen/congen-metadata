<!-- congen:begin body -->
# *Cyclopterus lumpus*

Population-genomic variant calls for 66 *Cyclopterus lumpus* samples ([NCBI taxon 8103](https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=8103)), produced by snpArcher against `GCA_009769545.1` and published on GenomeArk.

> [!NOTE]
> **PASS WITH WARNINGS** · validated 2026-09-10 · `GCA_009769545.1`
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
| Clade | fishes |
| Reference assembly | [`GCA_009769545.1`](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_009769545.1/) — fCycLum1.pri, Chromosome |
| Variant caller | gatk 4.6.2.0 |
| Samples | 66 across 68 sheet rows, all from SRA runs |
| Variant sites | 11,157,720 |

<details>
<summary>Assembly and pipeline detail</summary>

|  |  |
|---|---|
| Assembly organism | Cyclopterus lumpus |
| Paired RefSeq/GenBank accession | `GCF_009769545.1` |
| NCBI taxon | 8103 |
| Contigs in the VCF | 49 |
| Ploidy / heterozygosity prior | 2 / 0.005 |
| Other tools recorded in the VCF header | bcftools 1.23.1 |

</details>

## Sample QC

Full plots: [the snpArcher QC dashboard][qc/qc_dashboard.html]. Per-sample values are not reproduced here; they are in [`qc_report.tsv`][qc/qc_report.tsv], [`individuals.het`][qc/individuals.het] and [`individuals.imiss`][qc/individuals.imiss].

**Mean depth 13.6×** (IQR 13.2–15.7, range 11.2× `SAMN46524662` to 57.2× `SAMN23286145`), over mapped reads.

| mean depth | samples |  |
|---|---:|---|
| 10–15× | 46 | ██████████████████████████████ |
| 15–20× | 10 | ███████ |
| 20–30× | 9 | ██████ |
| 30–40× | 0 |  |
| 40–60× | 1 | █ |

Across the cohort:

|  |  |
|---|---|
| Cohort mean coverage | 15.2× |
| Reads mapped | median 98.9% (range 98.2–99.1%) |
| Duplicates | median 11.8% (range 7.6–18.4%) |
| Properly paired | median 97.3% (range 76.3–97.7%) |
| Missingness F_MISS | median 0.168 (range 0.130–0.268) |
| Inbreeding coefficient F | median 0.049 (range -0.080–0.099) |

The callable-sites mask was built over depths 7–31× ([`coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv]). That is a site-level threshold for the mask, not a per-sample cutoff: samples outside it are neither excluded nor flagged here.

## References

> [!CAUTION]
> **This dataset cannot yet be cited.**
>
> The recorded list of contributing BioProjects is known to be incorrect, so it is not reproduced here — a wrong citation list is worse than none, because it propagates into other people's papers. Do not publish analyses of this dataset until it is corrected.
>
> Reported by `E002` and `E003` — see [`VALIDATION.md`](VALIDATION.md) for what these mean and how to fix it.

## Getting the data

Everything below is under `s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_009769545.1/`. The bucket is public and needs no credentials, but the AWS CLI needs `--no-sign-request` or it will try to sign the request and fail.

|  | Size |  |
|---|---:|---|
| [`raw.vcf.gz`][vcfs/raw.vcf.gz] | 4.4 GiB | unfiltered joint-genotyped calls |
| [`raw.vcf.gz.tbi`][vcfs/raw.vcf.gz.tbi] | 472.1 KiB | index for the above |
| [`callable_sites.bed`][callable_sites/callable_sites.bed] | 2.6 MiB | the callable-sites mask |
| [`qc_dashboard.html`][qc/qc_dashboard.html] | 9.7 MiB | the snpArcher QC report — opens in a browser |
| `bams/` | 277.4 GiB | 132 objects — alignments and indexes |

This run did not produce `filtered.vcf.gz`.

```bash
# one region of the VCF, without downloading the whole thing
bcftools view -r <chr>:<start>-<end> \
  https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_009769545.1/vcfs/raw.vcf.gz

# the alignments — check the size above first
aws s3 sync --no-sign-request \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_009769545.1/bams/ ./bams/

# the masks and QC tables, without the zarr stores
aws s3 sync --no-sign-request --exclude '*.zarr/*' \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_009769545.1/callable_sites/ ./callable_sites/
```

> [!WARNING]
> `callable_sites/` also holds `callable_loci.zarr/`, `depths/`, `depths.zarr/` and `genmap_index/` — zarr stores of thousands of small objects each.
>
> Nothing here lists or sizes them, so the sizes above are a lower bound, and a recursive `sync` without the `--exclude` above will pull all of them.

<details>
<summary>The other 10 published files</summary>

|  | Size |
|---|---:|
| [`qc/qc_report.tsv`][qc/qc_report.tsv] | 5.9 KiB |
| [`qc/individuals.idepth`][qc/individuals.idepth] | 1.9 KiB |
| [`qc/individuals.het`][qc/individuals.het] | 3.1 KiB |
| [`qc/individuals.imiss`][qc/individuals.imiss] | 2.7 KiB |
| [`qc/individuals.samps.txt`][qc/individuals.samps.txt] | 858 B |
| [`qc/contig_map.tsv`][qc/contig_map.tsv] | 1.4 KiB |
| [`callable_sites/coverage.bed`][callable_sites/coverage.bed] | 25.2 MiB |
| [`callable_sites/mappability.bed`][callable_sites/mappability.bed] | 2.2 MiB |
| [`callable_sites/mappability.bedgraph`][callable_sites/mappability.bedgraph] | 318.4 MiB |
| [`callable_sites/coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv] | 62 B |

</details>

[callable_sites/callable_sites.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_009769545.1/callable_sites/callable_sites.bed
[callable_sites/coverage.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_009769545.1/callable_sites/coverage.bed
[callable_sites/coverage_thresholds.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_009769545.1/callable_sites/coverage_thresholds.tsv
[callable_sites/mappability.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_009769545.1/callable_sites/mappability.bed
[callable_sites/mappability.bedgraph]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_009769545.1/callable_sites/mappability.bedgraph
[qc/contig_map.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_009769545.1/qc/contig_map.tsv
[qc/individuals.het]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_009769545.1/qc/individuals.het
[qc/individuals.idepth]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_009769545.1/qc/individuals.idepth
[qc/individuals.imiss]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_009769545.1/qc/individuals.imiss
[qc/individuals.samps.txt]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_009769545.1/qc/individuals.samps.txt
[qc/qc_dashboard.html]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_009769545.1/qc/qc_dashboard.html
[qc/qc_report.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_009769545.1/qc/qc_report.tsv
[vcfs/raw.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_009769545.1/vcfs/raw.vcf.gz
[vcfs/raw.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_009769545.1/vcfs/raw.vcf.gz.tbi
<!-- congen:end body -->
