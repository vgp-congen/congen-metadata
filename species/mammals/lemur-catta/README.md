<!-- congen:begin body -->
# *Lemur catta*

Population-genomic variant calls for 12 *Lemur catta* samples ([NCBI taxon 9447](https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=9447)), produced by snpArcher against `GCA_020740605.1` and published on GenomeArk.

> [!NOTE]
> **PASS WITH WARNINGS** · validated 2026-09-10 · `GCA_020740605.1`
>
> Metadata agrees with the data published on GenomeArk, with 1 warning to note.
>
> **Provenance incomplete** — no reviewed list of contributing BioProjects exists for this species, so it cannot yet be cited. (`G017`)
>
> See [References](#references).
>
> Full report: [`VALIDATION.md`](VALIDATION.md) · check definitions: [`CHECKS.md`](../../../CHECKS.md) · baseline description: [`README.txt`](README.txt)

## Dataset

|  |  |
|---|---|
| Clade | mammals |
| Reference assembly | [`GCA_020740605.1`](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_020740605.1/) — mLemCat1.pri, Chromosome |
| Variant caller | gatk 4.6.2.0 |
| Samples | 12 across 35 sheet rows, all from SRA runs |
| Variant sites | 21,229,036 |

<details>
<summary>Assembly and pipeline detail</summary>

|  |  |
|---|---|
| Assembly organism | Lemur catta |
| Paired RefSeq/GenBank accession | `GCF_020740605.2` |
| NCBI taxon | 9447 |
| Contigs in the VCF | 185 |
| Ploidy / heterozygosity prior | 2 / 0.005 |
| Other tools recorded in the VCF header | bcftools 1.23.1 |

</details>

## Sample QC

Full plots: [the snpArcher QC dashboard][qc/qc_dashboard.html]. Per-sample values are not reproduced here; they are in [`qc_report.tsv`][qc/qc_report.tsv], [`individuals.het`][qc/individuals.het] and [`individuals.imiss`][qc/individuals.imiss].

**Mean depth 25.0×** (IQR 23.5–31.1, range 21.3× `SAMEA5533103` to 48.1× `SAMEA112483149`), over mapped reads.

| mean depth | samples |  |
|---|---:|---|
| 20–30× | 8 | ██████████████████████████████ |
| 30–40× | 3 | ███████████ |
| 40–60× | 1 | ████ |

Across the cohort:

|  |  |
|---|---|
| Cohort mean coverage | 27.2× |
| Reads mapped | median 99.2% (range 98.5–99.2%) |
| Duplicates | median 18.5% (range 11.2–31.0%) |
| Properly paired | median 97.6% (range 86.9–97.7%) |
| Missingness F_MISS | median 0.033 (range 0.023–0.165) |
| Inbreeding coefficient F | median -0.184 (range -0.430–-0.130) |

The callable-sites mask was built over depths 13–55× ([`coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv]). That is a site-level threshold for the mask, not a per-sample cutoff: samples outside it are neither excluded nor flagged here.

## References

> [!WARNING]
> **This dataset cannot yet be cited.**
>
> No reviewed list of contributing BioProjects exists in this repository, so there is nothing here to credit the people who generated the reads. Do not publish analyses of this dataset until that list exists.
>
> Reported by `G017` — see [`VALIDATION.md`](VALIDATION.md) for what that means and how to fix it.

## Getting the data

Everything below is under `s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_020740605.1/`. The bucket is public and needs no credentials, but the AWS CLI needs `--no-sign-request` or it will try to sign the request and fail.

|  | Size |  |
|---|---:|---|
| [`raw.vcf.gz`][vcfs/raw.vcf.gz] | 2.7 GiB | unfiltered joint-genotyped calls |
| [`raw.vcf.gz.tbi`][vcfs/raw.vcf.gz.tbi] | 1.7 MiB | index for the above |
| [`filtered.vcf.gz`][vcfs/filtered.vcf.gz] | 3.4 GiB | filtered calls |
| [`filtered.vcf.gz.tbi`][vcfs/filtered.vcf.gz.tbi] | 1.8 MiB | index for the above |
| [`callable_sites.bed`][callable_sites/callable_sites.bed] | 7.9 MiB | the callable-sites mask |
| [`qc_dashboard.html`][qc/qc_dashboard.html] | 8.4 MiB | the snpArcher QC report — opens in a browser |
| `bams/` | 405.4 GiB | 24 objects — alignments and indexes |

```bash
# one region of the VCF, without downloading the whole thing
bcftools view -r <chr>:<start>-<end> \
  https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_020740605.1/vcfs/raw.vcf.gz

# the alignments — check the size above first
aws s3 sync --no-sign-request \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_020740605.1/bams/ ./bams/

# the masks and QC tables, without the zarr stores
aws s3 sync --no-sign-request --exclude '*.zarr/*' \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_020740605.1/callable_sites/ ./callable_sites/
```

> [!WARNING]
> `callable_sites/` also holds `callable_loci.zarr/`, `depths/`, `depths.zarr/` and `genmap_index/` — zarr stores of thousands of small objects each.
>
> Nothing here lists or sizes them, so the sizes above are a lower bound, and a recursive `sync` without the `--exclude` above will pull all of them.

<details>
<summary>The other 10 published files</summary>

|  | Size |
|---|---:|
| [`qc/qc_report.tsv`][qc/qc_report.tsv] | 1.3 KiB |
| [`qc/individuals.idepth`][qc/individuals.idepth] | 385 B |
| [`qc/individuals.het`][qc/individuals.het] | 642 B |
| [`qc/individuals.imiss`][qc/individuals.imiss] | 541 B |
| [`qc/individuals.samps.txt`][qc/individuals.samps.txt] | 158 B |
| [`qc/contig_map.tsv`][qc/contig_map.tsv] | 6.8 KiB |
| [`callable_sites/coverage.bed`][callable_sites/coverage.bed] | 14.0 MiB |
| [`callable_sites/mappability.bed`][callable_sites/mappability.bed] | 3.6 MiB |
| [`callable_sites/mappability.bedgraph`][callable_sites/mappability.bedgraph] | 2.7 GiB |
| [`callable_sites/coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv] | 63 B |

</details>

[callable_sites/callable_sites.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_020740605.1/callable_sites/callable_sites.bed
[callable_sites/coverage.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_020740605.1/callable_sites/coverage.bed
[callable_sites/coverage_thresholds.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_020740605.1/callable_sites/coverage_thresholds.tsv
[callable_sites/mappability.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_020740605.1/callable_sites/mappability.bed
[callable_sites/mappability.bedgraph]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_020740605.1/callable_sites/mappability.bedgraph
[qc/contig_map.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_020740605.1/qc/contig_map.tsv
[qc/individuals.het]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_020740605.1/qc/individuals.het
[qc/individuals.idepth]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_020740605.1/qc/individuals.idepth
[qc/individuals.imiss]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_020740605.1/qc/individuals.imiss
[qc/individuals.samps.txt]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_020740605.1/qc/individuals.samps.txt
[qc/qc_dashboard.html]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_020740605.1/qc/qc_dashboard.html
[qc/qc_report.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_020740605.1/qc/qc_report.tsv
[vcfs/filtered.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_020740605.1/vcfs/filtered.vcf.gz
[vcfs/filtered.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_020740605.1/vcfs/filtered.vcf.gz.tbi
[vcfs/raw.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_020740605.1/vcfs/raw.vcf.gz
[vcfs/raw.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_020740605.1/vcfs/raw.vcf.gz.tbi
<!-- congen:end body -->
