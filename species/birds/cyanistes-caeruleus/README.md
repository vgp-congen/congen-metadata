<!-- congen:begin body -->
# *Cyanistes caeruleus*

Population-genomic variant calls for 76 *Cyanistes caeruleus* samples ([NCBI taxon 156563](https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=156563)), produced by snpArcher against `GCA_971803275.1` and published on GenomeArk.

> [!NOTE]
> **PASS WITH WARNINGS** · validated 2026-11-02 · `GCA_971803275.1`
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
| Reference assembly | [`GCA_971803275.1`](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_971803275.1/) — bCyaCae1.hap1.1, Chromosome |
| Variant caller | gatk 4.6.2.0 |
| Samples | 76, all from SRA runs |
| Variant sites | 61,775,415 |

<details>
<summary>Assembly and pipeline detail</summary>

|  |  |
|---|---|
| Assembly organism | Cyanistes caeruleus |
| Paired RefSeq/GenBank accession | `GCF_971803275.1` |
| NCBI taxon | 156563 |
| Contigs in the VCF | 456 |
| Ploidy / heterozygosity prior | 2 / 0.005 |
| Other tools recorded in the VCF header | bcftools 1.24 |

</details>

## Sample QC

Full plots: [the snpArcher QC dashboard][qc/qc_dashboard.html]. Per-sample values are not reproduced here; they are in [`qc_report.tsv`][qc/qc_report.tsv], [`individuals.het`][qc/individuals.het] and [`individuals.imiss`][qc/individuals.imiss].

**Mean depth 20.5×** (IQR 18.5–22.8, range 13.8× `SAMN21168690` to 50.2× `SAMN21168625`), over mapped reads.

| mean depth | samples |  |
|---|---:|---|
| 10–15× | 1 | █ |
| 15–20× | 34 | ██████████████████████████ |
| 20–30× | 39 | ██████████████████████████████ |
| 30–40× | 1 | █ |
| 40–60× | 1 | █ |

Across the cohort:

|  |  |
|---|---|
| Cohort mean coverage | 20.7× |
| Reads mapped | median 99.0% (range 98.4–99.7%) |
| Duplicates | median 12.8% (range 6.2–25.1%) |
| Properly paired | median 90.8% (range 87.2–96.9%) |
| Missingness F_MISS | median 0.254 (range 0.125–0.280) |
| Inbreeding coefficient F | median 0.004 (range -0.056–0.249) |

The callable-sites mask was built over depths 10–42× ([`coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv]). That is a site-level threshold for the mask, not a per-sample cutoff: samples outside it are neither excluded nor flagged here.

## References

> [!WARNING]
> **This dataset cannot yet be cited.**
>
> No reviewed list of contributing BioProjects exists in this repository, so there is nothing here to credit the people who generated the reads. Do not publish analyses of this dataset until that list exists.
>
> Reported by `G017` — see [`VALIDATION.md`](VALIDATION.md) for what that means and how to fix it.

## Getting the data

Everything below is under `s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_971803275.1/`. The bucket is public and needs no credentials, but the AWS CLI needs `--no-sign-request` or it will try to sign the request and fail.

|  | Size |  |
|---|---:|---|
| [`raw.vcf.gz`][vcfs/raw.vcf.gz] | 26.8 GiB | unfiltered joint-genotyped calls |
| [`raw.vcf.gz.tbi`][vcfs/raw.vcf.gz.tbi] | 1.1 MiB | index for the above |
| [`filtered.vcf.gz`][vcfs/filtered.vcf.gz] | 38.5 GiB | filtered calls |
| [`filtered.vcf.gz.tbi`][vcfs/filtered.vcf.gz.tbi] | 1.1 MiB | index for the above |
| [`callable_sites.bed`][callable_sites/callable_sites.bed] | 2.7 MiB | the callable-sites mask |
| [`qc_dashboard.html`][qc/qc_dashboard.html] | 9.9 MiB | the snpArcher QC report — opens in a browser |
| `bams/` | 1265.2 GiB | 152 objects — alignments and indexes |

```bash
# one region of the VCF, without downloading the whole thing
bcftools view -r <chr>:<start>-<end> \
  https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_971803275.1/vcfs/raw.vcf.gz

# the alignments — check the size above first
aws s3 sync --no-sign-request \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_971803275.1/bams/ ./bams/

# the masks and QC tables, without the zarr stores
aws s3 sync --no-sign-request --exclude '*.zarr/*' \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_971803275.1/callable_sites/ ./callable_sites/
```

> [!WARNING]
> `callable_sites/` also holds `callable_loci.zarr/`, `depths/`, `depths.zarr/` and `genmap_index/` — zarr stores of thousands of small objects each.
>
> Nothing here lists or sizes them, so the sizes above are a lower bound, and a recursive `sync` without the `--exclude` above will pull all of them.

<details>
<summary>The other 10 published files</summary>

|  | Size |
|---|---:|
| [`qc/qc_report.tsv`][qc/qc_report.tsv] | 7.1 KiB |
| [`qc/individuals.idepth`][qc/individuals.idepth] | 2.2 KiB |
| [`qc/individuals.het`][qc/individuals.het] | 3.8 KiB |
| [`qc/individuals.imiss`][qc/individuals.imiss] | 3.2 KiB |
| [`qc/individuals.samps.txt`][qc/individuals.samps.txt] | 988 B |
| [`qc/contig_map.tsv`][qc/contig_map.tsv] | 17.2 KiB |
| [`callable_sites/coverage.bed`][callable_sites/coverage.bed] | 38.3 MiB |
| [`callable_sites/mappability.bed`][callable_sites/mappability.bed] | 2.5 MiB |
| [`callable_sites/mappability.bedgraph`][callable_sites/mappability.bedgraph] | 2.1 GiB |
| [`callable_sites/coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv] | 63 B |

</details>

[callable_sites/callable_sites.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_971803275.1/callable_sites/callable_sites.bed
[callable_sites/coverage.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_971803275.1/callable_sites/coverage.bed
[callable_sites/coverage_thresholds.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_971803275.1/callable_sites/coverage_thresholds.tsv
[callable_sites/mappability.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_971803275.1/callable_sites/mappability.bed
[callable_sites/mappability.bedgraph]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_971803275.1/callable_sites/mappability.bedgraph
[qc/contig_map.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_971803275.1/qc/contig_map.tsv
[qc/individuals.het]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_971803275.1/qc/individuals.het
[qc/individuals.idepth]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_971803275.1/qc/individuals.idepth
[qc/individuals.imiss]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_971803275.1/qc/individuals.imiss
[qc/individuals.samps.txt]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_971803275.1/qc/individuals.samps.txt
[qc/qc_dashboard.html]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_971803275.1/qc/qc_dashboard.html
[qc/qc_report.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_971803275.1/qc/qc_report.tsv
[vcfs/filtered.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_971803275.1/vcfs/filtered.vcf.gz
[vcfs/filtered.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_971803275.1/vcfs/filtered.vcf.gz.tbi
[vcfs/raw.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_971803275.1/vcfs/raw.vcf.gz
[vcfs/raw.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_971803275.1/vcfs/raw.vcf.gz.tbi
<!-- congen:end body -->
