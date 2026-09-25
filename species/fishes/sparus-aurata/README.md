<!-- congen:begin body -->
# *Sparus aurata*

Population-genomic variant calls for 75 *Sparus aurata* samples ([NCBI taxon 8175](https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=8175)), produced by snpArcher against `GCA_900880675.2` and published on GenomeArk.

> [!NOTE]
> **PASS WITH WARNINGS** · validated 2026-09-25 · `GCA_900880675.2`
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
| Reference assembly | [`GCA_900880675.2`](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_900880675.2/) — fSpaAur1.2, Chromosome |
| Variant caller | gatk 4.6.2.0 |
| Samples | 75 across 78 sheet rows, all from SRA runs |
| Variant sites | 32,752,924 |

<details>
<summary>Assembly and pipeline detail</summary>

|  |  |
|---|---|
| Assembly organism | Sparus aurata |
| Paired RefSeq/GenBank accession | `GCF_900880675.2` |
| NCBI taxon | 8175 |
| Contigs in the VCF | 176 |
| Ploidy / heterozygosity prior | 2 / 0.005 |
| Other tools recorded in the VCF header | bcftools 1.24 |

</details>

## Sample QC

Full plots: [the snpArcher QC dashboard][qc/qc_dashboard.html]. Per-sample values are not reproduced here; they are in [`qc_report.tsv`][qc/qc_report.tsv], [`individuals.het`][qc/individuals.het] and [`individuals.imiss`][qc/individuals.imiss].

**Mean depth 60.0×** (IQR 30.0–64.6, range 10.8× `SAMN12172394` to 86.1× `SAMEA7325183`), over mapped reads.

| mean depth | samples |  |
|---|---:|---|
| 10–15× | 8 | ██████ |
| 15–20× | 10 | ████████ |
| 20–30× | 1 | █ |
| 30–40× | 3 | ██ |
| 40–60× | 15 | ████████████ |
| 60–100× | 38 | ██████████████████████████████ |

Across the cohort:

|  |  |
|---|---|
| Cohort mean coverage | 48.4× |
| Reads mapped | median 99.1% (range 90.0–99.5%) |
| Duplicates | median 13.5% (range 0.5–21.8%) |
| Properly paired | median 95.6% (range 80.4–97.0%) |
| Missingness F_MISS | median 0.035 (range 0.033–0.190) |
| Inbreeding coefficient F | median -0.626 (range -1.424–0.897) |

The callable-sites mask was built over depths 24–97× ([`coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv]). That is a site-level threshold for the mask, not a per-sample cutoff: samples outside it are neither excluded nor flagged here.

## References

> [!WARNING]
> **This dataset cannot yet be cited.**
>
> No reviewed list of contributing BioProjects exists in this repository, so there is nothing here to credit the people who generated the reads. Do not publish analyses of this dataset until that list exists.
>
> Reported by `G017` — see [`VALIDATION.md`](VALIDATION.md) for what that means and how to fix it.

## Getting the data

Everything below is under `s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_900880675.2/`. The bucket is public and needs no credentials, but the AWS CLI needs `--no-sign-request` or it will try to sign the request and fail.

|  | Size |  |
|---|---:|---|
| [`raw.vcf.gz`][vcfs/raw.vcf.gz] | 17.3 GiB | unfiltered joint-genotyped calls |
| [`raw.vcf.gz.tbi`][vcfs/raw.vcf.gz.tbi] | 783.3 KiB | index for the above |
| [`filtered.vcf.gz`][vcfs/filtered.vcf.gz] | 22.0 GiB | filtered calls |
| [`filtered.vcf.gz.tbi`][vcfs/filtered.vcf.gz.tbi] | 784.1 KiB | index for the above |
| [`callable_sites.bed`][callable_sites/callable_sites.bed] | 4.0 MiB | the callable-sites mask |
| [`qc_dashboard.html`][qc/qc_dashboard.html] | 9.8 MiB | the snpArcher QC report — opens in a browser |
| `bams/` | 1714.6 GiB | 150 objects — alignments and indexes |

```bash
# one region of the VCF, without downloading the whole thing
bcftools view -r <chr>:<start>-<end> \
  https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_900880675.2/vcfs/raw.vcf.gz

# the alignments — check the size above first
aws s3 sync --no-sign-request \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_900880675.2/bams/ ./bams/

# the masks and QC tables, without the zarr stores
aws s3 sync --no-sign-request --exclude '*.zarr/*' \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_900880675.2/callable_sites/ ./callable_sites/
```

> [!WARNING]
> `callable_sites/` also holds `callable_loci.zarr/`, `depths/`, `depths.zarr/` and `genmap_index/` — zarr stores of thousands of small objects each.
>
> Nothing here lists or sizes them, so the sizes above are a lower bound, and a recursive `sync` without the `--exclude` above will pull all of them.

<details>
<summary>The other 10 published files</summary>

|  | Size |
|---|---:|
| [`qc/qc_report.tsv`][qc/qc_report.tsv] | 6.9 KiB |
| [`qc/individuals.idepth`][qc/individuals.idepth] | 2.2 KiB |
| [`qc/individuals.het`][qc/individuals.het] | 3.7 KiB |
| [`qc/individuals.imiss`][qc/individuals.imiss] | 3.1 KiB |
| [`qc/individuals.samps.txt`][qc/individuals.samps.txt] | 977 B |
| [`qc/contig_map.tsv`][qc/contig_map.tsv] | 6.5 KiB |
| [`callable_sites/coverage.bed`][callable_sites/coverage.bed] | 251 B |
| [`callable_sites/mappability.bed`][callable_sites/mappability.bed] | 4.0 MiB |
| [`callable_sites/mappability.bedgraph`][callable_sites/mappability.bedgraph] | 317.5 MiB |
| [`callable_sites/coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv] | 63 B |

</details>

[callable_sites/callable_sites.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_900880675.2/callable_sites/callable_sites.bed
[callable_sites/coverage.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_900880675.2/callable_sites/coverage.bed
[callable_sites/coverage_thresholds.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_900880675.2/callable_sites/coverage_thresholds.tsv
[callable_sites/mappability.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_900880675.2/callable_sites/mappability.bed
[callable_sites/mappability.bedgraph]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_900880675.2/callable_sites/mappability.bedgraph
[qc/contig_map.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_900880675.2/qc/contig_map.tsv
[qc/individuals.het]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_900880675.2/qc/individuals.het
[qc/individuals.idepth]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_900880675.2/qc/individuals.idepth
[qc/individuals.imiss]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_900880675.2/qc/individuals.imiss
[qc/individuals.samps.txt]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_900880675.2/qc/individuals.samps.txt
[qc/qc_dashboard.html]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_900880675.2/qc/qc_dashboard.html
[qc/qc_report.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_900880675.2/qc/qc_report.tsv
[vcfs/filtered.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_900880675.2/vcfs/filtered.vcf.gz
[vcfs/filtered.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_900880675.2/vcfs/filtered.vcf.gz.tbi
[vcfs/raw.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_900880675.2/vcfs/raw.vcf.gz
[vcfs/raw.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_900880675.2/vcfs/raw.vcf.gz.tbi
<!-- congen:end body -->
