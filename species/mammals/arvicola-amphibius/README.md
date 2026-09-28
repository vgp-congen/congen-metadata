<!-- congen:begin body -->
# *Arvicola amphibius*

Population-genomic variant calls for 29 *Arvicola amphibius* samples ([NCBI taxon 1047088](https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=1047088)), produced by snpArcher against `GCA_903992535.2` and published on GenomeArk.

> [!NOTE]
> **PASS WITH WARNINGS** · validated 2026-09-28 · `GCA_903992535.2`
>
> Metadata agrees with the data published on GenomeArk, with 1 warning to note.
>
> **Provenance incorrect** — the recorded list of contributing BioProjects is known to be wrong, so it cannot be used for citation. (`E002`)
>
> See [References](#references).
>
> Full report: [`VALIDATION.md`](VALIDATION.md) · check definitions: [`CHECKS.md`](../../../CHECKS.md) · baseline description: [`README.txt`](README.txt)

## Dataset

|  |  |
|---|---|
| Clade | mammals |
| Reference assembly | [`GCA_903992535.2`](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_903992535.2/) — mArvAmp1.2, Chromosome |
| Variant caller | gatk 4.6.2.0 |
| Samples | 29 across 31 sheet rows, all from SRA runs |
| Variant sites | 33,740,898 |

<details>
<summary>Assembly and pipeline detail</summary>

|  |  |
|---|---|
| Assembly organism | Arvicola amphibius |
| Paired RefSeq/GenBank accession | `GCF_903992535.2` |
| NCBI taxon | 1047088 |
| Contigs in the VCF | 215 |
| Ploidy / heterozygosity prior | 2 / 0.005 |
| Other tools recorded in the VCF header | bcftools 1.23.1 |

</details>

## Sample QC

Per-sample values are not reproduced here; they are in [`qc_report.tsv`][qc/qc_report.tsv], [`individuals.het`][qc/individuals.het] and [`individuals.imiss`][qc/individuals.imiss].

**Mean depth 23.8×** (IQR 22.9–26.6, range 17.9× `SAMEA118840307` to 27.8× `SAMEA117818188`), over mapped reads.

| mean depth | samples |  |
|---|---:|---|
| 15–20× | 6 | ████████ |
| 20–30× | 23 | ██████████████████████████████ |

Across the cohort:

|  |  |
|---|---|
| Cohort mean coverage | 19.5× |
| Reads mapped | median 99.7% (range 97.2–99.8%) |
| Duplicates | median 12.1% (range 9.2–13.7%) |
| Properly paired | median 96.2% (range 88.6–97.0%) |
| Missingness F_MISS | median 0.043 (range 0.028–0.160) |
| Inbreeding coefficient F | median 0.387 (range -0.235–0.469) |

The callable-sites mask was built over depths 9–40× ([`coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv]). That is a site-level threshold for the mask, not a per-sample cutoff: samples outside it are neither excluded nor flagged here.

## References

> [!CAUTION]
> **This dataset cannot yet be cited.**
>
> The recorded list of contributing BioProjects is known to be incorrect, so it is not reproduced here — a wrong citation list is worse than none, because it propagates into other people's papers. Do not publish analyses of this dataset until it is corrected.
>
> Reported by `E002` — see [`VALIDATION.md`](VALIDATION.md) for what that means and how to fix it.

## Getting the data

Everything below is under `s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_903992535.2/`. The bucket is public and needs no credentials, but the AWS CLI needs `--no-sign-request` or it will try to sign the request and fail.

|  | Size |  |
|---|---:|---|
| [`raw.vcf.gz`][vcfs/raw.vcf.gz] | 7.2 GiB | unfiltered joint-genotyped calls |
| [`raw.vcf.gz.tbi`][vcfs/raw.vcf.gz.tbi] | 2.0 MiB | index for the above |
| [`callable_sites.bed`][callable_sites/callable_sites.bed] | 4.7 MiB | the callable-sites mask |
| `bams/` | 672.6 GiB | 58 objects — alignments and indexes |

This run did not produce `filtered.vcf.gz` and `qc_dashboard.html`.

```bash
# one region of the VCF, without downloading the whole thing
bcftools view -r <chr>:<start>-<end> \
  https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_903992535.2/vcfs/raw.vcf.gz

# the alignments — check the size above first
aws s3 sync --no-sign-request \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_903992535.2/bams/ ./bams/

# the masks and QC tables, without the zarr stores
aws s3 sync --no-sign-request --exclude '*.zarr/*' \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_903992535.2/callable_sites/ ./callable_sites/
```

> [!WARNING]
> `callable_sites/` also holds `callable_loci.zarr/`, `depths/`, `depths.zarr/` and `genmap_index/` — zarr stores of thousands of small objects each.
>
> Nothing here lists or sizes them, so the sizes above are a lower bound, and a recursive `sync` without the `--exclude` above will pull all of them.

<details>
<summary>The other 10 published files</summary>

|  | Size |
|---|---:|
| [`qc/qc_report.tsv`][qc/qc_report.tsv] | 2.9 KiB |
| [`qc/individuals.idepth`][qc/individuals.idepth] | 946 B |
| [`qc/individuals.het`][qc/individuals.het] | 1.5 KiB |
| [`qc/individuals.imiss`][qc/individuals.imiss] | 1.3 KiB |
| [`qc/individuals.samps.txt`][qc/individuals.samps.txt] | 432 B |
| [`qc/contig_map.tsv`][qc/contig_map.tsv] | 8.1 KiB |
| [`callable_sites/coverage.bed`][callable_sites/coverage.bed] | 37.9 MiB |
| [`callable_sites/mappability.bed`][callable_sites/mappability.bed] | 3.8 MiB |
| [`callable_sites/mappability.bedgraph`][callable_sites/mappability.bedgraph] | 1.1 GiB |
| [`callable_sites/coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv] | 62 B |

</details>

[callable_sites/callable_sites.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_903992535.2/callable_sites/callable_sites.bed
[callable_sites/coverage.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_903992535.2/callable_sites/coverage.bed
[callable_sites/coverage_thresholds.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_903992535.2/callable_sites/coverage_thresholds.tsv
[callable_sites/mappability.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_903992535.2/callable_sites/mappability.bed
[callable_sites/mappability.bedgraph]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_903992535.2/callable_sites/mappability.bedgraph
[qc/contig_map.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_903992535.2/qc/contig_map.tsv
[qc/individuals.het]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_903992535.2/qc/individuals.het
[qc/individuals.idepth]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_903992535.2/qc/individuals.idepth
[qc/individuals.imiss]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_903992535.2/qc/individuals.imiss
[qc/individuals.samps.txt]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_903992535.2/qc/individuals.samps.txt
[qc/qc_report.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_903992535.2/qc/qc_report.tsv
[vcfs/raw.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_903992535.2/vcfs/raw.vcf.gz
[vcfs/raw.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_903992535.2/vcfs/raw.vcf.gz.tbi
<!-- congen:end body -->
