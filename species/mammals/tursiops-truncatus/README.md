<!-- congen:begin body -->
# *Tursiops truncatus*

Population-genomic variant calls for 65 *Tursiops truncatus* samples ([NCBI taxon 9739](https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=9739)), produced by snpArcher against `GCA_011762595.2` and published on GenomeArk.

> [!NOTE]
> **PASS WITH WARNINGS** · validated 2026-09-25 · `GCA_011762595.2`
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
| Reference assembly | [`GCA_011762595.2`](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_011762595.2/) — mTurTru1.mat.Y, Chromosome |
| Variant caller | gatk 4.6.2.0 |
| Samples | 65 across 71 sheet rows, all from SRA runs |
| Variant sites | 44,820,036 |

<details>
<summary>Assembly and pipeline detail</summary>

|  |  |
|---|---|
| Assembly organism | Tursiops truncatus |
| Paired RefSeq/GenBank accession | `GCF_011762595.2` |
| NCBI taxon | 9739 |
| Contigs in the VCF | 362 |
| Ploidy / heterozygosity prior | 2 / 0.005 |
| Other tools recorded in the VCF header | bcftools 1.24 |

</details>

## Sample QC

Full plots: [the snpArcher QC dashboard][qc/qc_dashboard.html]. Per-sample values are not reproduced here; they are in [`qc_report.tsv`][qc/qc_report.tsv], [`individuals.het`][qc/individuals.het] and [`individuals.imiss`][qc/individuals.imiss].

**Mean depth 11.9×** (IQR 10.2–13.4, range 8.3× `SAMN18839493` to 52.6× `SAMN09426418`), over mapped reads.

| mean depth | samples |  |
|---|---:|---|
| 5–10× | 15 | ███████████ |
| 10–15× | 42 | ██████████████████████████████ |
| 15–20× | 3 | ██ |
| 20–30× | 3 | ██ |
| 30–40× | 0 |  |
| 40–60× | 2 | █ |

Across the cohort:

|  |  |
|---|---|
| Cohort mean coverage | 12.9× |
| Reads mapped | median 99.9% (range 98.8–99.9%) |
| Duplicates | median 23.1% (range 2.1–42.7%) |
| Properly paired | median 97.2% (range 78.8–99.3%) |
| Missingness F_MISS | median 0.047 (range 0.024–0.260) |
| Inbreeding coefficient F | median 0.145 (range -0.051–0.555) |

The callable-sites mask was built over depths 6–26× ([`coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv]). That is a site-level threshold for the mask, not a per-sample cutoff: samples outside it are neither excluded nor flagged here.

## References

> [!WARNING]
> **This dataset cannot yet be cited.**
>
> No reviewed list of contributing BioProjects exists in this repository, so there is nothing here to credit the people who generated the reads. Do not publish analyses of this dataset until that list exists.
>
> Reported by `G017` — see [`VALIDATION.md`](VALIDATION.md) for what that means and how to fix it.

## Getting the data

Everything below is under `s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_011762595.2/`. The bucket is public and needs no credentials, but the AWS CLI needs `--no-sign-request` or it will try to sign the request and fail.

|  | Size |  |
|---|---:|---|
| [`raw.vcf.gz`][vcfs/raw.vcf.gz] | 15.2 GiB | unfiltered joint-genotyped calls |
| [`raw.vcf.gz.tbi`][vcfs/raw.vcf.gz.tbi] | 2.2 MiB | index for the above |
| [`filtered.vcf.gz`][vcfs/filtered.vcf.gz] | 21.6 GiB | filtered calls |
| [`filtered.vcf.gz.tbi`][vcfs/filtered.vcf.gz.tbi] | 2.2 MiB | index for the above |
| [`callable_sites.bed`][callable_sites/callable_sites.bed] | 3.3 MiB | the callable-sites mask |
| [`qc_dashboard.html`][qc/qc_dashboard.html] | 9.6 MiB | the snpArcher QC report — opens in a browser |
| `bams/` | 1143.0 GiB | 130 objects — alignments and indexes |

```bash
# one region of the VCF, without downloading the whole thing
bcftools view -r <chr>:<start>-<end> \
  https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_011762595.2/vcfs/raw.vcf.gz

# the alignments — check the size above first
aws s3 sync --no-sign-request \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_011762595.2/bams/ ./bams/

# the masks and QC tables, without the zarr stores
aws s3 sync --no-sign-request --exclude '*.zarr/*' \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_011762595.2/callable_sites/ ./callable_sites/
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
| [`qc/individuals.het`][qc/individuals.het] | 3.2 KiB |
| [`qc/individuals.imiss`][qc/individuals.imiss] | 2.7 KiB |
| [`qc/individuals.samps.txt`][qc/individuals.samps.txt] | 845 B |
| [`qc/contig_map.tsv`][qc/contig_map.tsv] | 13.7 KiB |
| [`callable_sites/coverage.bed`][callable_sites/coverage.bed] | 2.4 MiB |
| [`callable_sites/mappability.bed`][callable_sites/mappability.bed] | 3.3 MiB |
| [`callable_sites/mappability.bedgraph`][callable_sites/mappability.bedgraph] | 1.2 GiB |
| [`callable_sites/coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv] | 62 B |

</details>

[callable_sites/callable_sites.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_011762595.2/callable_sites/callable_sites.bed
[callable_sites/coverage.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_011762595.2/callable_sites/coverage.bed
[callable_sites/coverage_thresholds.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_011762595.2/callable_sites/coverage_thresholds.tsv
[callable_sites/mappability.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_011762595.2/callable_sites/mappability.bed
[callable_sites/mappability.bedgraph]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_011762595.2/callable_sites/mappability.bedgraph
[qc/contig_map.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_011762595.2/qc/contig_map.tsv
[qc/individuals.het]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_011762595.2/qc/individuals.het
[qc/individuals.idepth]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_011762595.2/qc/individuals.idepth
[qc/individuals.imiss]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_011762595.2/qc/individuals.imiss
[qc/individuals.samps.txt]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_011762595.2/qc/individuals.samps.txt
[qc/qc_dashboard.html]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_011762595.2/qc/qc_dashboard.html
[qc/qc_report.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_011762595.2/qc/qc_report.tsv
[vcfs/filtered.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_011762595.2/vcfs/filtered.vcf.gz
[vcfs/filtered.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_011762595.2/vcfs/filtered.vcf.gz.tbi
[vcfs/raw.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_011762595.2/vcfs/raw.vcf.gz
[vcfs/raw.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_011762595.2/vcfs/raw.vcf.gz.tbi
<!-- congen:end body -->
