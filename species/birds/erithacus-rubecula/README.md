<!-- congen:begin body -->
# *Erithacus rubecula*

Population-genomic variant calls for 65 *Erithacus rubecula* samples ([NCBI taxon 37610](https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=37610)), produced by snpArcher against `GCA_903797595.2` and published on GenomeArk.

> [!NOTE]
> **PASS WITH WARNINGS** · validated 2026-11-02 · `GCA_903797595.2`
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
| Reference assembly | [`GCA_903797595.2`](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_903797595.2/) — bEriRub2.2, Chromosome |
| Variant caller | gatk 4.6.2.0 |
| Samples | 65 across 259 sheet rows, all from SRA runs |
| Variant sites | 44,238,724 |

<details>
<summary>Assembly and pipeline detail</summary>

|  |  |
|---|---|
| Assembly organism | Erithacus rubecula |
| Paired RefSeq/GenBank accession | `GCF_903797595.1` |
| NCBI taxon | 37610 |
| Contigs in the VCF | 1119 |
| Ploidy / heterozygosity prior | 2 / 0.005 |
| Other tools recorded in the VCF header | bcftools 1.23.1 |

</details>

## Sample QC

Full plots: [the snpArcher QC dashboard][qc/qc_dashboard.html]. Per-sample values are not reproduced here; they are in [`qc_report.tsv`][qc/qc_report.tsv], [`individuals.het`][qc/individuals.het] and [`individuals.imiss`][qc/individuals.imiss].

**Mean depth 31.2×** (IQR 27.3–36.0, range 7.3× `SAMEA115094667` to 77.4× `SAMEA4760689`), over mapped reads.

| mean depth | samples |  |
|---|---:|---|
| 5–10× | 1 | █ |
| 10–15× | 0 |  |
| 15–20× | 1 | █ |
| 20–30× | 28 | ██████████████████████████████ |
| 30–40× | 22 | ████████████████████████ |
| 40–60× | 12 | █████████████ |
| 60–100× | 1 | █ |

Across the cohort:

|  |  |
|---|---|
| Cohort mean coverage | 31.7× |
| Reads mapped | median 98.9% (range 98.4–99.1%) |
| Duplicates | median 16.5% (range 9.8–55.3%) |
| Properly paired | median 96.9% (range 87.1–97.4%) |
| Missingness F_MISS | median 0.051 (range 0.038–0.203) |
| Inbreeding coefficient F | median 0.001 (range -0.177–0.182) |

The callable-sites mask was built over depths 15–64× ([`coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv]). That is a site-level threshold for the mask, not a per-sample cutoff: samples outside it are neither excluded nor flagged here.

## References

> [!WARNING]
> **This dataset cannot yet be cited.**
>
> No reviewed list of contributing BioProjects exists in this repository, so there is nothing here to credit the people who generated the reads. Do not publish analyses of this dataset until that list exists.
>
> Reported by `G017` — see [`VALIDATION.md`](VALIDATION.md) for what that means and how to fix it.

## Getting the data

Everything below is under `s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_903797595.2/`. The bucket is public and needs no credentials, but the AWS CLI needs `--no-sign-request` or it will try to sign the request and fail.

|  | Size |  |
|---|---:|---|
| [`raw.vcf.gz`][vcfs/raw.vcf.gz] | 19.1 GiB | unfiltered joint-genotyped calls |
| [`raw.vcf.gz.tbi`][vcfs/raw.vcf.gz.tbi] | 960.6 KiB | index for the above |
| [`filtered.vcf.gz`][vcfs/filtered.vcf.gz] | 26.2 GiB | filtered calls |
| [`filtered.vcf.gz.tbi`][vcfs/filtered.vcf.gz.tbi] | 959.7 KiB | index for the above |
| [`callable_sites.bed`][callable_sites/callable_sites.bed] | 3.2 MiB | the callable-sites mask |
| [`qc_dashboard.html`][qc/qc_dashboard.html] | 9.6 MiB | the snpArcher QC report — opens in a browser |
| `bams/` | 1085.5 GiB | 130 objects — alignments and indexes |

```bash
# one region of the VCF, without downloading the whole thing
bcftools view -r <chr>:<start>-<end> \
  https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_903797595.2/vcfs/raw.vcf.gz

# the alignments — check the size above first
aws s3 sync --no-sign-request \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_903797595.2/bams/ ./bams/

# the masks and QC tables, without the zarr stores
aws s3 sync --no-sign-request --exclude '*.zarr/*' \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_903797595.2/callable_sites/ ./callable_sites/
```

> [!WARNING]
> `callable_sites/` also holds `callable_loci.zarr/`, `depths/`, `depths.zarr/` and `genmap_index/` — zarr stores of thousands of small objects each.
>
> Nothing here lists or sizes them, so the sizes above are a lower bound, and a recursive `sync` without the `--exclude` above will pull all of them.

<details>
<summary>The other 10 published files</summary>

|  | Size |
|---|---:|
| [`qc/qc_report.tsv`][qc/qc_report.tsv] | 6.3 KiB |
| [`qc/individuals.idepth`][qc/individuals.idepth] | 2.0 KiB |
| [`qc/individuals.het`][qc/individuals.het] | 3.4 KiB |
| [`qc/individuals.imiss`][qc/individuals.imiss] | 2.8 KiB |
| [`qc/individuals.samps.txt`][qc/individuals.samps.txt] | 971 B |
| [`qc/contig_map.tsv`][qc/contig_map.tsv] | 43.3 KiB |
| [`callable_sites/coverage.bed`][callable_sites/coverage.bed] | 64.5 KiB |
| [`callable_sites/mappability.bed`][callable_sites/mappability.bed] | 3.2 MiB |
| [`callable_sites/mappability.bedgraph`][callable_sites/mappability.bedgraph] | 281.8 MiB |
| [`callable_sites/coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv] | 63 B |

</details>

[callable_sites/callable_sites.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_903797595.2/callable_sites/callable_sites.bed
[callable_sites/coverage.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_903797595.2/callable_sites/coverage.bed
[callable_sites/coverage_thresholds.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_903797595.2/callable_sites/coverage_thresholds.tsv
[callable_sites/mappability.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_903797595.2/callable_sites/mappability.bed
[callable_sites/mappability.bedgraph]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_903797595.2/callable_sites/mappability.bedgraph
[qc/contig_map.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_903797595.2/qc/contig_map.tsv
[qc/individuals.het]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_903797595.2/qc/individuals.het
[qc/individuals.idepth]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_903797595.2/qc/individuals.idepth
[qc/individuals.imiss]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_903797595.2/qc/individuals.imiss
[qc/individuals.samps.txt]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_903797595.2/qc/individuals.samps.txt
[qc/qc_dashboard.html]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_903797595.2/qc/qc_dashboard.html
[qc/qc_report.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_903797595.2/qc/qc_report.tsv
[vcfs/filtered.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_903797595.2/vcfs/filtered.vcf.gz
[vcfs/filtered.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_903797595.2/vcfs/filtered.vcf.gz.tbi
[vcfs/raw.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_903797595.2/vcfs/raw.vcf.gz
[vcfs/raw.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_903797595.2/vcfs/raw.vcf.gz.tbi
<!-- congen:end body -->
