<!-- congen:begin body -->
# *Hirundo rustica*

Population-genomic variant calls for 149 *Hirundo rustica* samples ([NCBI taxon 43150](https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=43150)), produced by snpArcher against `GCA_015227805.3` and published on GenomeArk.

> [!NOTE]
> **PASS WITH WARNINGS** · validated 2026-09-10 · `GCA_015227805.3`
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
| Clade | birds |
| Reference assembly | [`GCA_015227805.3`](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_015227805.3/) — bHirRus1.pri.v3, Chromosome |
| Variant caller | gatk 4.6.2.0 |
| Samples | 149 across 152 sheet rows, all from SRA runs |
| Variant sites | 117,188,679 |

<details>
<summary>Assembly and pipeline detail</summary>

|  |  |
|---|---|
| Assembly organism | Hirundo rustica |
| Paired RefSeq/GenBank accession | `GCF_015227805.2` |
| NCBI taxon | 43150 |
| Contigs in the VCF | 617 |
| Ploidy / heterozygosity prior | 2 / 0.005 |
| Other tools recorded in the VCF header | bcftools 1.23.1 |

</details>

## Sample QC

Full plots: [the snpArcher QC dashboard][qc/qc_dashboard.html]. Per-sample values are not reproduced here; they are in [`qc_report.tsv`][qc/qc_report.tsv], [`individuals.het`][qc/individuals.het] and [`individuals.imiss`][qc/individuals.imiss].

**Mean depth 10.9×** (IQR 9.6–13.3, range 1.1× `SAMN43946467` to 29.8× `SAMN43946423`), over mapped reads.

| mean depth | samples |  |
|---|---:|---|
| 0–5× | 3 | █ |
| 5–10× | 47 | ███████████████████ |
| 10–15× | 76 | ██████████████████████████████ |
| 15–20× | 15 | ██████ |
| 20–30× | 8 | ███ |

Across the cohort:

|  |  |
|---|---|
| Cohort mean coverage | 10.3× |
| Reads mapped | median 98.8% (range 96.8–99.3%) |
| Duplicates | median 27.4% (range 14.3–86.5%) |
| Properly paired | median 97.1% (range 89.0–98.2%) |
| Missingness F_MISS | median 0.099 (range 0.033–0.636) |
| Inbreeding coefficient F | median 0.163 (range -0.104–0.837) |

The callable-sites mask was built over depths 5–21× ([`coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv]). That is a site-level threshold for the mask, not a per-sample cutoff: samples outside it are neither excluded nor flagged here.

## References

> [!WARNING]
> **This dataset cannot yet be cited.**
>
> No reviewed list of contributing BioProjects exists in this repository, so there is nothing here to credit the people who generated the reads. Do not publish analyses of this dataset until that list exists.
>
> Reported by `G017` — see [`VALIDATION.md`](VALIDATION.md) for what that means and how to fix it.

## Getting the data

Everything below is under `s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_015227805.3/`. The bucket is public and needs no credentials, but the AWS CLI needs `--no-sign-request` or it will try to sign the request and fail.

|  | Size |  |
|---|---:|---|
| [`raw.vcf.gz`][vcfs/raw.vcf.gz] | 69.1 GiB | unfiltered joint-genotyped calls |
| [`raw.vcf.gz.tbi`][vcfs/raw.vcf.gz.tbi] | 1004.8 KiB | index for the above |
| [`callable_sites.bed`][callable_sites/callable_sites.bed] | 2.0 MiB | the callable-sites mask |
| [`qc_dashboard.html`][qc/qc_dashboard.html] | 12.3 MiB | the snpArcher QC report — opens in a browser |
| `bams/` | 973.4 GiB | 298 objects — alignments and indexes |

This run did not produce `filtered.vcf.gz`.

```bash
# one region of the VCF, without downloading the whole thing
bcftools view -r <chr>:<start>-<end> \
  https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_015227805.3/vcfs/raw.vcf.gz

# the alignments — check the size above first
aws s3 sync --no-sign-request \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_015227805.3/bams/ ./bams/

# the masks and QC tables, without the zarr stores
aws s3 sync --no-sign-request --exclude '*.zarr/*' \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_015227805.3/callable_sites/ ./callable_sites/
```

> [!WARNING]
> `callable_sites/` also holds `callable_loci.zarr/`, `depths/`, `depths.zarr/` and `genmap_index/` — zarr stores of thousands of small objects each.
>
> Nothing here lists or sizes them, so the sizes above are a lower bound, and a recursive `sync` without the `--exclude` above will pull all of them.

<details>
<summary>The other 10 published files</summary>

|  | Size |
|---|---:|
| [`qc/qc_report.tsv`][qc/qc_report.tsv] | 13.6 KiB |
| [`qc/individuals.idepth`][qc/individuals.idepth] | 4.5 KiB |
| [`qc/individuals.het`][qc/individuals.het] | 7.3 KiB |
| [`qc/individuals.imiss`][qc/individuals.imiss] | 6.3 KiB |
| [`qc/individuals.samps.txt`][qc/individuals.samps.txt] | 1.9 KiB |
| [`qc/contig_map.tsv`][qc/contig_map.tsv] | 23.5 KiB |
| [`callable_sites/coverage.bed`][callable_sites/coverage.bed] | 0 B |
| [`callable_sites/mappability.bed`][callable_sites/mappability.bed] | 2.0 MiB |
| [`callable_sites/mappability.bedgraph`][callable_sites/mappability.bedgraph] | 201.5 MiB |
| [`callable_sites/coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv] | 62 B |

</details>

[callable_sites/callable_sites.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_015227805.3/callable_sites/callable_sites.bed
[callable_sites/coverage.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_015227805.3/callable_sites/coverage.bed
[callable_sites/coverage_thresholds.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_015227805.3/callable_sites/coverage_thresholds.tsv
[callable_sites/mappability.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_015227805.3/callable_sites/mappability.bed
[callable_sites/mappability.bedgraph]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_015227805.3/callable_sites/mappability.bedgraph
[qc/contig_map.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_015227805.3/qc/contig_map.tsv
[qc/individuals.het]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_015227805.3/qc/individuals.het
[qc/individuals.idepth]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_015227805.3/qc/individuals.idepth
[qc/individuals.imiss]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_015227805.3/qc/individuals.imiss
[qc/individuals.samps.txt]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_015227805.3/qc/individuals.samps.txt
[qc/qc_dashboard.html]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_015227805.3/qc/qc_dashboard.html
[qc/qc_report.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_015227805.3/qc/qc_report.tsv
[vcfs/raw.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_015227805.3/vcfs/raw.vcf.gz
[vcfs/raw.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_015227805.3/vcfs/raw.vcf.gz.tbi
<!-- congen:end body -->
