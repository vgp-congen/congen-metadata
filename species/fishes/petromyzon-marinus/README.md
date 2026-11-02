<!-- congen:begin body -->
# *Petromyzon marinus*

Population-genomic variant calls for 14 *Petromyzon marinus* samples ([NCBI taxon 7757](https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=7757)), produced by snpArcher against `GCA_048934315.1` and published on GenomeArk.

> [!NOTE]
> **PASS WITH WARNINGS** · validated 2026-11-02 · `GCA_048934315.1`
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
| Reference assembly | [`GCA_048934315.1`](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_048934315.1/) — UKy_Petmar_22M1.pri1.0, Chromosome |
| Variant caller | gatk 4.6.2.0 |
| Samples | 14, all from SRA runs |
| Variant sites | 32,564,742 |

<details>
<summary>Assembly and pipeline detail</summary>

|  |  |
|---|---|
| Assembly organism | Petromyzon marinus |
| Paired RefSeq/GenBank accession | `GCF_048934315.1` |
| NCBI taxon | 7757 |
| Contigs in the VCF | 1203 |
| Ploidy / heterozygosity prior | 2 / 0.005 |
| Other tools recorded in the VCF header | bcftools 1.24 |

</details>

## Sample QC

Full plots: [the snpArcher QC dashboard][qc/qc_dashboard.html]. Per-sample values are not reproduced here; they are in [`qc_report.tsv`][qc/qc_report.tsv], [`individuals.het`][qc/individuals.het] and [`individuals.imiss`][qc/individuals.imiss].

**Mean depth 57.6×** (IQR 5.3–91.2, range 2.4× `SAMN55037059` to 119.0× `SAMN37687927`), over mapped reads.

| mean depth | samples |  |
|---|---:|---|
| 0–5× | 4 | ██████████████████████████████ |
| 5–10× | 0 |  |
| 10–15× | 1 | ████████ |
| 15–20× | 1 | ████████ |
| 20–30× | 0 |  |
| 30–40× | 0 |  |
| 40–60× | 2 | ███████████████ |
| 60–100× | 4 | ██████████████████████████████ |
| 100×+ | 2 | ███████████████ |

Across the cohort:

|  |  |
|---|---|
| Cohort mean coverage | 39.6× |
| Reads mapped | median 99.8% (range 99.2–99.9%) |
| Duplicates | median 6.0% (range 3.4–17.5%) |
| Properly paired | median 95.5% (range 94.0–97.6%) |
| Missingness F_MISS | median 0.105 (range 0.093–0.455) |
| Inbreeding coefficient F | median 0.347 (range -0.054–0.698) |

The callable-sites mask was built over depths 19–80× ([`coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv]). That is a site-level threshold for the mask, not a per-sample cutoff: samples outside it are neither excluded nor flagged here.

## References

> [!WARNING]
> **This dataset cannot yet be cited.**
>
> No reviewed list of contributing BioProjects exists in this repository, so there is nothing here to credit the people who generated the reads. Do not publish analyses of this dataset until that list exists.
>
> Reported by `G017` — see [`VALIDATION.md`](VALIDATION.md) for what that means and how to fix it.

## Getting the data

Everything below is under `s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_048934315.1/`. The bucket is public and needs no credentials, but the AWS CLI needs `--no-sign-request` or it will try to sign the request and fail.

|  | Size |  |
|---|---:|---|
| [`raw.vcf.gz`][vcfs/raw.vcf.gz] | 4.1 GiB | unfiltered joint-genotyped calls |
| [`raw.vcf.gz.tbi`][vcfs/raw.vcf.gz.tbi] | 1.2 MiB | index for the above |
| [`filtered.vcf.gz`][vcfs/filtered.vcf.gz] | 5.3 GiB | filtered calls |
| [`filtered.vcf.gz.tbi`][vcfs/filtered.vcf.gz.tbi] | 1.2 MiB | index for the above |
| [`callable_sites.bed`][callable_sites/callable_sites.bed] | 14.1 MiB | the callable-sites mask |
| [`qc_dashboard.html`][qc/qc_dashboard.html] | 8.5 MiB | the snpArcher QC report — opens in a browser |
| `bams/` | 538.0 GiB | 28 objects — alignments and indexes |

```bash
# one region of the VCF, without downloading the whole thing
bcftools view -r <chr>:<start>-<end> \
  https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_048934315.1/vcfs/raw.vcf.gz

# the alignments — check the size above first
aws s3 sync --no-sign-request \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_048934315.1/bams/ ./bams/

# the masks and QC tables, without the zarr stores
aws s3 sync --no-sign-request --exclude '*.zarr/*' \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_048934315.1/callable_sites/ ./callable_sites/
```

> [!WARNING]
> `callable_sites/` also holds `callable_loci.zarr/`, `depths/`, `depths.zarr/` and `genmap_index/` — zarr stores of thousands of small objects each.
>
> Nothing here lists or sizes them, so the sizes above are a lower bound, and a recursive `sync` without the `--exclude` above will pull all of them.

<details>
<summary>The other 10 published files</summary>

|  | Size |
|---|---:|
| [`qc/qc_report.tsv`][qc/qc_report.tsv] | 1.4 KiB |
| [`qc/individuals.idepth`][qc/individuals.idepth] | 443 B |
| [`qc/individuals.het`][qc/individuals.het] | 730 B |
| [`qc/individuals.imiss`][qc/individuals.imiss] | 626 B |
| [`qc/individuals.samps.txt`][qc/individuals.samps.txt] | 182 B |
| [`qc/contig_map.tsv`][qc/contig_map.tsv] | 46.0 KiB |
| [`callable_sites/coverage.bed`][callable_sites/coverage.bed] | 242.6 KiB |
| [`callable_sites/mappability.bed`][callable_sites/mappability.bed] | 13.9 MiB |
| [`callable_sites/mappability.bedgraph`][callable_sites/mappability.bedgraph] | 9.5 GiB |
| [`callable_sites/coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv] | 63 B |

</details>

[callable_sites/callable_sites.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_048934315.1/callable_sites/callable_sites.bed
[callable_sites/coverage.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_048934315.1/callable_sites/coverage.bed
[callable_sites/coverage_thresholds.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_048934315.1/callable_sites/coverage_thresholds.tsv
[callable_sites/mappability.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_048934315.1/callable_sites/mappability.bed
[callable_sites/mappability.bedgraph]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_048934315.1/callable_sites/mappability.bedgraph
[qc/contig_map.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_048934315.1/qc/contig_map.tsv
[qc/individuals.het]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_048934315.1/qc/individuals.het
[qc/individuals.idepth]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_048934315.1/qc/individuals.idepth
[qc/individuals.imiss]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_048934315.1/qc/individuals.imiss
[qc/individuals.samps.txt]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_048934315.1/qc/individuals.samps.txt
[qc/qc_dashboard.html]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_048934315.1/qc/qc_dashboard.html
[qc/qc_report.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_048934315.1/qc/qc_report.tsv
[vcfs/filtered.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_048934315.1/vcfs/filtered.vcf.gz
[vcfs/filtered.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_048934315.1/vcfs/filtered.vcf.gz.tbi
[vcfs/raw.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_048934315.1/vcfs/raw.vcf.gz
[vcfs/raw.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_048934315.1/vcfs/raw.vcf.gz.tbi
<!-- congen:end body -->
