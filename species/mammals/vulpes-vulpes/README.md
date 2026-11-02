<!-- congen:begin body -->
# *Vulpes vulpes*

Population-genomic variant calls for 43 *Vulpes vulpes* samples ([NCBI taxon 9627](https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=9627)), produced by snpArcher against `GCA_964106825.2` and published on GenomeArk.

> [!NOTE]
> **PASS WITH WARNINGS** · validated 2026-11-02 · `GCA_964106825.2`
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
| Reference assembly | [`GCA_964106825.2`](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_964106825.2/) — mVulVul1.hap1.2, Chromosome |
| Variant caller | gatk 4.6.2.0 |
| Samples | 43 across 69 sheet rows, all from SRA runs |
| Variant sites | 52,036,066 |

<details>
<summary>Assembly and pipeline detail</summary>

|  |  |
|---|---|
| Assembly organism | Vulpes vulpes |
| NCBI taxon | 9627 |
| Contigs in the VCF | 461 |
| Ploidy / heterozygosity prior | 2 / 0.005 |
| Other tools recorded in the VCF header | bcftools 1.24 |

</details>

## Sample QC

Full plots: [the snpArcher QC dashboard][qc/qc_dashboard.html]. Per-sample values are not reproduced here; they are in [`qc_report.tsv`][qc/qc_report.tsv], [`individuals.het`][qc/individuals.het] and [`individuals.imiss`][qc/individuals.imiss].

**Mean depth 18.0×** (IQR 14.2–23.2, range 7.9× `SAMN43533626` to 64.0× `SAMN47229189`), over mapped reads.

| mean depth | samples |  |
|---|---:|---|
| 5–10× | 4 | ████████ |
| 10–15× | 9 | ██████████████████ |
| 15–20× | 15 | ██████████████████████████████ |
| 20–30× | 10 | ████████████████████ |
| 30–40× | 0 |  |
| 40–60× | 4 | ████████ |
| 60–100× | 1 | ██ |

Across the cohort:

|  |  |
|---|---|
| Cohort mean coverage | 20.4× |
| Reads mapped | median 99.7% (range 92.6–99.8%) |
| Duplicates | median 16.7% (range 11.0–67.7%) |
| Properly paired | median 97.6% (range 75.1–98.6%) |
| Missingness F_MISS | median 0.036 (range 0.024–0.118) |
| Inbreeding coefficient F | median 0.343 (range -0.091–0.557) |

The callable-sites mask was built over depths 10–41× ([`coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv]). That is a site-level threshold for the mask, not a per-sample cutoff: samples outside it are neither excluded nor flagged here.

## References

> [!WARNING]
> **This dataset cannot yet be cited.**
>
> No reviewed list of contributing BioProjects exists in this repository, so there is nothing here to credit the people who generated the reads. Do not publish analyses of this dataset until that list exists.
>
> Reported by `G017` — see [`VALIDATION.md`](VALIDATION.md) for what that means and how to fix it.

## Getting the data

Everything below is under `s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_964106825.2/`. The bucket is public and needs no credentials, but the AWS CLI needs `--no-sign-request` or it will try to sign the request and fail.

|  | Size |  |
|---|---:|---|
| [`raw.vcf.gz`][vcfs/raw.vcf.gz] | 16.4 GiB | unfiltered joint-genotyped calls |
| [`raw.vcf.gz.tbi`][vcfs/raw.vcf.gz.tbi] | 2.2 MiB | index for the above |
| [`filtered.vcf.gz`][vcfs/filtered.vcf.gz] | 22.1 GiB | filtered calls |
| [`filtered.vcf.gz.tbi`][vcfs/filtered.vcf.gz.tbi] | 2.2 MiB | index for the above |
| [`callable_sites.bed`][callable_sites/callable_sites.bed] | 3.3 MiB | the callable-sites mask |
| [`qc_dashboard.html`][qc/qc_dashboard.html] | 9.1 MiB | the snpArcher QC report — opens in a browser |
| `bams/` | 1022.8 GiB | 86 objects — alignments and indexes |

```bash
# one region of the VCF, without downloading the whole thing
bcftools view -r <chr>:<start>-<end> \
  https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_964106825.2/vcfs/raw.vcf.gz

# the alignments — check the size above first
aws s3 sync --no-sign-request \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_964106825.2/bams/ ./bams/

# the masks and QC tables, without the zarr stores
aws s3 sync --no-sign-request --exclude '*.zarr/*' \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_964106825.2/callable_sites/ ./callable_sites/
```

> [!WARNING]
> `callable_sites/` also holds `callable_loci.zarr/`, `depths/`, `depths.zarr/` and `genmap_index/` — zarr stores of thousands of small objects each.
>
> Nothing here lists or sizes them, so the sizes above are a lower bound, and a recursive `sync` without the `--exclude` above will pull all of them.

<details>
<summary>The other 10 published files</summary>

|  | Size |
|---|---:|
| [`qc/qc_report.tsv`][qc/qc_report.tsv] | 4.1 KiB |
| [`qc/individuals.idepth`][qc/individuals.idepth] | 1.3 KiB |
| [`qc/individuals.het`][qc/individuals.het] | 2.1 KiB |
| [`qc/individuals.imiss`][qc/individuals.imiss] | 1.8 KiB |
| [`qc/individuals.samps.txt`][qc/individuals.samps.txt] | 559 B |
| [`qc/contig_map.tsv`][qc/contig_map.tsv] | 17.7 KiB |
| [`callable_sites/coverage.bed`][callable_sites/coverage.bed] | 108.4 KiB |
| [`callable_sites/mappability.bed`][callable_sites/mappability.bed] | 3.3 MiB |
| [`callable_sites/mappability.bedgraph`][callable_sites/mappability.bedgraph] | 783.1 MiB |
| [`callable_sites/coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv] | 63 B |

</details>

[callable_sites/callable_sites.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_964106825.2/callable_sites/callable_sites.bed
[callable_sites/coverage.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_964106825.2/callable_sites/coverage.bed
[callable_sites/coverage_thresholds.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_964106825.2/callable_sites/coverage_thresholds.tsv
[callable_sites/mappability.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_964106825.2/callable_sites/mappability.bed
[callable_sites/mappability.bedgraph]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_964106825.2/callable_sites/mappability.bedgraph
[qc/contig_map.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_964106825.2/qc/contig_map.tsv
[qc/individuals.het]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_964106825.2/qc/individuals.het
[qc/individuals.idepth]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_964106825.2/qc/individuals.idepth
[qc/individuals.imiss]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_964106825.2/qc/individuals.imiss
[qc/individuals.samps.txt]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_964106825.2/qc/individuals.samps.txt
[qc/qc_dashboard.html]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_964106825.2/qc/qc_dashboard.html
[qc/qc_report.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_964106825.2/qc/qc_report.tsv
[vcfs/filtered.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_964106825.2/vcfs/filtered.vcf.gz
[vcfs/filtered.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_964106825.2/vcfs/filtered.vcf.gz.tbi
[vcfs/raw.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_964106825.2/vcfs/raw.vcf.gz
[vcfs/raw.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_964106825.2/vcfs/raw.vcf.gz.tbi
<!-- congen:end body -->
