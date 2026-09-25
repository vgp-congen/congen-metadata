<!-- congen:begin body -->
# *Monodon monoceros*

Population-genomic variant calls for 65 *Monodon monoceros* samples ([NCBI taxon 40151](https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=40151)), produced by snpArcher against `GCA_005190385.4` and published on GenomeArk.

> [!NOTE]
> **PASS WITH WARNINGS** · validated 2026-09-25 · `GCA_005190385.4`
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
| Reference assembly | [`GCA_005190385.4`](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_005190385.4/) — mMonMon2.pri, Chromosome |
| Variant caller | gatk 4.6.2.0 |
| Samples | 65 across 67 sheet rows, all from SRA runs |
| Variant sites | 15,785,487 |

<details>
<summary>Assembly and pipeline detail</summary>

|  |  |
|---|---|
| Assembly organism | Monodon monoceros |
| NCBI taxon | 40151 |
| Contigs in the VCF | 91 |
| Ploidy / heterozygosity prior | 2 / 0.005 |
| Other tools recorded in the VCF header | bcftools 1.24 |

</details>

## Sample QC

Full plots: [the snpArcher QC dashboard][qc/qc_dashboard.html]. Per-sample values are not reproduced here; they are in [`qc_report.tsv`][qc/qc_report.tsv], [`individuals.het`][qc/individuals.het] and [`individuals.imiss`][qc/individuals.imiss].

**Mean depth 19.2×** (IQR 16.8–23.2, range 1.6× `SAMN10519625` to 61.1× `SAMN29855413`), over mapped reads.

| mean depth | samples |  |
|---|---:|---|
| 0–5× | 1 | █ |
| 5–10× | 2 | ██ |
| 10–15× | 5 | █████ |
| 15–20× | 30 | ██████████████████████████████ |
| 20–30× | 25 | █████████████████████████ |
| 30–40× | 0 |  |
| 40–60× | 1 | █ |
| 60–100× | 1 | █ |

Across the cohort:

|  |  |
|---|---|
| Cohort mean coverage | 19.8× |
| Reads mapped | median 99.6% (range 97.8–99.7%) |
| Duplicates | median 7.8% (range 1.8–77.3%) |
| Properly paired | median 97.8% (range 69.3–98.2%) |
| Missingness F_MISS | median 0.056 (range 0.032–0.631) |
| Inbreeding coefficient F | median -0.042 (range -1.327–0.773) |

The callable-sites mask was built over depths 9–40× ([`coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv]). That is a site-level threshold for the mask, not a per-sample cutoff: samples outside it are neither excluded nor flagged here.

## References

> [!WARNING]
> **This dataset cannot yet be cited.**
>
> No reviewed list of contributing BioProjects exists in this repository, so there is nothing here to credit the people who generated the reads. Do not publish analyses of this dataset until that list exists.
>
> Reported by `G017` — see [`VALIDATION.md`](VALIDATION.md) for what that means and how to fix it.

## Getting the data

Everything below is under `s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_005190385.4/`. The bucket is public and needs no credentials, but the AWS CLI needs `--no-sign-request` or it will try to sign the request and fail.

|  | Size |  |
|---|---:|---|
| [`raw.vcf.gz`][vcfs/raw.vcf.gz] | 6.2 GiB | unfiltered joint-genotyped calls |
| [`raw.vcf.gz.tbi`][vcfs/raw.vcf.gz.tbi] | 2.0 MiB | index for the above |
| [`filtered.vcf.gz`][vcfs/filtered.vcf.gz] | 8.7 GiB | filtered calls |
| [`filtered.vcf.gz.tbi`][vcfs/filtered.vcf.gz.tbi] | 2.1 MiB | index for the above |
| [`callable_sites.bed`][callable_sites/callable_sites.bed] | 2.8 MiB | the callable-sites mask |
| [`qc_dashboard.html`][qc/qc_dashboard.html] | 9.7 MiB | the snpArcher QC report — opens in a browser |
| `bams/` | 1083.8 GiB | 130 objects — alignments and indexes |

```bash
# one region of the VCF, without downloading the whole thing
bcftools view -r <chr>:<start>-<end> \
  https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_005190385.4/vcfs/raw.vcf.gz

# the alignments — check the size above first
aws s3 sync --no-sign-request \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_005190385.4/bams/ ./bams/

# the masks and QC tables, without the zarr stores
aws s3 sync --no-sign-request --exclude '*.zarr/*' \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_005190385.4/callable_sites/ ./callable_sites/
```

> [!WARNING]
> `callable_sites/` also holds `callable_loci.zarr/`, `depths/`, `depths.zarr/` and `genmap_index/` — zarr stores of thousands of small objects each.
>
> Nothing here lists or sizes them, so the sizes above are a lower bound, and a recursive `sync` without the `--exclude` above will pull all of them.

<details>
<summary>The other 10 published files</summary>

|  | Size |
|---|---:|
| [`qc/qc_report.tsv`][qc/qc_report.tsv] | 6.1 KiB |
| [`qc/individuals.idepth`][qc/individuals.idepth] | 1.9 KiB |
| [`qc/individuals.het`][qc/individuals.het] | 3.3 KiB |
| [`qc/individuals.imiss`][qc/individuals.imiss] | 2.6 KiB |
| [`qc/individuals.samps.txt`][qc/individuals.samps.txt] | 845 B |
| [`qc/contig_map.tsv`][qc/contig_map.tsv] | 2.8 KiB |
| [`callable_sites/coverage.bed`][callable_sites/coverage.bed] | 998.0 KiB |
| [`callable_sites/mappability.bed`][callable_sites/mappability.bed] | 2.7 MiB |
| [`callable_sites/mappability.bedgraph`][callable_sites/mappability.bedgraph] | 638.9 MiB |
| [`callable_sites/coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv] | 62 B |

</details>

[callable_sites/callable_sites.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_005190385.4/callable_sites/callable_sites.bed
[callable_sites/coverage.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_005190385.4/callable_sites/coverage.bed
[callable_sites/coverage_thresholds.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_005190385.4/callable_sites/coverage_thresholds.tsv
[callable_sites/mappability.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_005190385.4/callable_sites/mappability.bed
[callable_sites/mappability.bedgraph]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_005190385.4/callable_sites/mappability.bedgraph
[qc/contig_map.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_005190385.4/qc/contig_map.tsv
[qc/individuals.het]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_005190385.4/qc/individuals.het
[qc/individuals.idepth]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_005190385.4/qc/individuals.idepth
[qc/individuals.imiss]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_005190385.4/qc/individuals.imiss
[qc/individuals.samps.txt]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_005190385.4/qc/individuals.samps.txt
[qc/qc_dashboard.html]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_005190385.4/qc/qc_dashboard.html
[qc/qc_report.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_005190385.4/qc/qc_report.tsv
[vcfs/filtered.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_005190385.4/vcfs/filtered.vcf.gz
[vcfs/filtered.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_005190385.4/vcfs/filtered.vcf.gz.tbi
[vcfs/raw.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_005190385.4/vcfs/raw.vcf.gz
[vcfs/raw.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_005190385.4/vcfs/raw.vcf.gz.tbi
<!-- congen:end body -->
