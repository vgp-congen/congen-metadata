<!-- congen:begin body -->
# *Coregonus lavaretus*

Population-genomic variant calls for 61 *Coregonus lavaretus* samples ([NCBI taxon 59291](https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=59291)), produced by snpArcher against `GCA_964263955.1` and published on GenomeArk.

> [!NOTE]
> **PASS WITH WARNINGS** · validated 2026-09-10 · `GCA_964263955.1`
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
| Clade | fishes |
| Reference assembly | [`GCA_964263955.1`](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_964263955.1/) — fCorLav1.hap1.1, Chromosome |
| Variant caller | gatk 4.6.2.0 |
| Samples | 61, all from SRA runs |
| Variant sites | 282,029,782 |

<details>
<summary>Assembly and pipeline detail</summary>

|  |  |
|---|---|
| Assembly organism | Coregonus lavaretus |
| Paired RefSeq/GenBank accession | `GCF_964263955.1` |
| NCBI taxon | 59291 |
| Contigs in the VCF | 11150 |
| Ploidy / heterozygosity prior | 2 / 0.005 |
| Other tools recorded in the VCF header | bcftools 1.23.1 |

</details>

## Sample QC

Full plots: [the snpArcher QC dashboard][qc/qc_dashboard.html]. Per-sample values are not reproduced here; they are in [`qc_report.tsv`][qc/qc_report.tsv], [`individuals.het`][qc/individuals.het] and [`individuals.imiss`][qc/individuals.imiss].

**Mean depth 18.2×** (IQR 4.4–18.8, range 2.9× `SAMN12560632` to 22.7× `SAMEA13927290`), over mapped reads.

| mean depth | samples |  |
|---|---:|---|
| 0–5× | 25 | ████████████████████████ |
| 5–10× | 4 | ████ |
| 10–15× | 0 |  |
| 15–20× | 31 | ██████████████████████████████ |
| 20–30× | 1 | █ |

Across the cohort:

|  |  |
|---|---|
| Cohort mean coverage | 10.3× |
| Reads mapped | median 99.2% (range 93.8–99.5%) |
| Duplicates | median 5.2% (range 0.4–13.4%) |
| Properly paired | median 91.8% (range 87.5–95.8%) |
| Missingness F_MISS | median 0.068 (range 0.051–0.665) |
| Inbreeding coefficient F | median 0.846 (range 0.671–0.892) |

The callable-sites mask was built over depths 5–21× ([`coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv]). That is a site-level threshold for the mask, not a per-sample cutoff: samples outside it are neither excluded nor flagged here.

## References

> [!WARNING]
> **This dataset cannot yet be cited.**
>
> No reviewed list of contributing BioProjects exists in this repository, so there is nothing here to credit the people who generated the reads. Do not publish analyses of this dataset until that list exists.
>
> Reported by `G017` — see [`VALIDATION.md`](VALIDATION.md) for what that means and how to fix it.

## Getting the data

Everything below is under `s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_964263955.1/`. The bucket is public and needs no credentials, but the AWS CLI needs `--no-sign-request` or it will try to sign the request and fail.

|  | Size |  |
|---|---:|---|
| [`raw.vcf.gz`][vcfs/raw.vcf.gz] | 59.0 GiB | unfiltered joint-genotyped calls |
| [`raw.vcf.gz.tbi`][vcfs/raw.vcf.gz.tbi] | 2.5 MiB | index for the above |
| [`filtered.vcf.gz`][vcfs/filtered.vcf.gz] | 89.2 GiB | filtered calls |
| [`filtered.vcf.gz.tbi`][vcfs/filtered.vcf.gz.tbi] | 2.5 MiB | index for the above |
| [`callable_sites.bed`][callable_sites/callable_sites.bed] | 39.2 MiB | the callable-sites mask |
| [`qc_dashboard.html`][qc/qc_dashboard.html] | 9.5 MiB | the snpArcher QC report — opens in a browser |
| `bams/` | 1938.1 GiB | 122 objects — alignments and indexes |

```bash
# one region of the VCF, without downloading the whole thing
bcftools view -r <chr>:<start>-<end> \
  https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_964263955.1/vcfs/raw.vcf.gz

# the alignments — check the size above first
aws s3 sync --no-sign-request \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_964263955.1/bams/ ./bams/

# the masks and QC tables, without the zarr stores
aws s3 sync --no-sign-request --exclude '*.zarr/*' \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_964263955.1/callable_sites/ ./callable_sites/
```

> [!WARNING]
> `callable_sites/` also holds `callable_loci.zarr/`, `depths/`, `depths.zarr/` and `genmap_index/` — zarr stores of thousands of small objects each.
>
> Nothing here lists or sizes them, so the sizes above are a lower bound, and a recursive `sync` without the `--exclude` above will pull all of them.

<details>
<summary>The other 10 published files</summary>

|  | Size |
|---|---:|
| [`qc/qc_report.tsv`][qc/qc_report.tsv] | 5.7 KiB |
| [`qc/individuals.idepth`][qc/individuals.idepth] | 1.9 KiB |
| [`qc/individuals.het`][qc/individuals.het] | 3.1 KiB |
| [`qc/individuals.imiss`][qc/individuals.imiss] | 2.7 KiB |
| [`qc/individuals.samps.txt`][qc/individuals.samps.txt] | 795 B |
| [`qc/contig_map.tsv`][qc/contig_map.tsv] | 446.0 KiB |
| [`callable_sites/coverage.bed`][callable_sites/coverage.bed] | 635.5 KiB |
| [`callable_sites/mappability.bed`][callable_sites/mappability.bed] | 38.9 MiB |
| [`callable_sites/mappability.bedgraph`][callable_sites/mappability.bedgraph] | 13.8 GiB |
| [`callable_sites/coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv] | 62 B |

</details>

[callable_sites/callable_sites.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_964263955.1/callable_sites/callable_sites.bed
[callable_sites/coverage.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_964263955.1/callable_sites/coverage.bed
[callable_sites/coverage_thresholds.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_964263955.1/callable_sites/coverage_thresholds.tsv
[callable_sites/mappability.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_964263955.1/callable_sites/mappability.bed
[callable_sites/mappability.bedgraph]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_964263955.1/callable_sites/mappability.bedgraph
[qc/contig_map.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_964263955.1/qc/contig_map.tsv
[qc/individuals.het]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_964263955.1/qc/individuals.het
[qc/individuals.idepth]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_964263955.1/qc/individuals.idepth
[qc/individuals.imiss]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_964263955.1/qc/individuals.imiss
[qc/individuals.samps.txt]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_964263955.1/qc/individuals.samps.txt
[qc/qc_dashboard.html]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_964263955.1/qc/qc_dashboard.html
[qc/qc_report.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_964263955.1/qc/qc_report.tsv
[vcfs/filtered.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_964263955.1/vcfs/filtered.vcf.gz
[vcfs/filtered.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_964263955.1/vcfs/filtered.vcf.gz.tbi
[vcfs/raw.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_964263955.1/vcfs/raw.vcf.gz
[vcfs/raw.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_964263955.1/vcfs/raw.vcf.gz.tbi
<!-- congen:end body -->
