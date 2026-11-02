<!-- congen:begin body -->
# *Cuculus canorus*

Population-genomic variant calls for 323 *Cuculus canorus* samples ([NCBI taxon 55661](https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=55661)), produced by snpArcher against `GCA_017976375.1` and published on GenomeArk.

> [!NOTE]
> **PASS WITH WARNINGS** · validated 2026-11-02 · `GCA_017976375.1`
>
> Metadata agrees with the data published on GenomeArk, with 1 warning to note.
>
> **Provenance incorrect** — the recorded list of contributing BioProjects is known to be wrong, so it cannot be used for citation. (`E002`)
>
> See [References](#references).
>
> Full report: [`VALIDATION.md`](VALIDATION.md) · check definitions: [`CHECKS.md`](../../../CHECKS.md) · baseline description: [`README.txt`](README.txt)

## Dataset

|  |  |
|---|---|
| Clade | birds |
| Reference assembly | [`GCA_017976375.1`](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_017976375.1/) — bCucCan1.pri, Chromosome |
| Variant caller | gatk 4.6.2.0 |
| Samples | 323 across 530 sheet rows, all from SRA runs |
| Variant sites | 121,907,737 |

<details>
<summary>Assembly and pipeline detail</summary>

|  |  |
|---|---|
| Assembly organism | Cuculus canorus |
| Paired RefSeq/GenBank accession | `GCF_017976375.1` |
| NCBI taxon | 55661 |
| Contigs in the VCF | 151 |
| Ploidy / heterozygosity prior | 2 / 0.005 |
| Other tools recorded in the VCF header | bcftools 1.24 |

</details>

## Sample QC

Full plots: [the snpArcher QC dashboard][qc/qc_dashboard.html]. Per-sample values are not reproduced here; they are in [`qc_report.tsv`][qc/qc_report.tsv], [`individuals.het`][qc/individuals.het] and [`individuals.imiss`][qc/individuals.imiss].

**Mean depth 13.4×** (IQR 11.5–15.0, range 5.2× `SAMN35327862` to 90.2× `SAMN02298149`), over mapped reads.

| mean depth | samples |  |
|---|---:|---|
| 5–10× | 51 | ████████ |
| 10–15× | 192 | ██████████████████████████████ |
| 15–20× | 71 | ███████████ |
| 20–30× | 4 | █ |
| 30–40× | 2 | █ |
| 40–60× | 0 |  |
| 60–100× | 3 | █ |

Across the cohort:

|  |  |
|---|---|
| Cohort mean coverage | 12.9× |
| Reads mapped | median 99.5% (range 69.4–99.9%) |
| Duplicates | median 21.6% (range 0.2–47.5%) |
| Properly paired | median 97.4% (range 67.2–99.1%) |
| Missingness F_MISS | median 0.041 (range 0.025–0.207) |
| Inbreeding coefficient F | median 0.099 (range -0.741–0.419) |

The callable-sites mask was built over depths 6–26× ([`coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv]). That is a site-level threshold for the mask, not a per-sample cutoff: samples outside it are neither excluded nor flagged here.

## References

> [!CAUTION]
> **This dataset cannot yet be cited.**
>
> The recorded list of contributing BioProjects is known to be incorrect, so it is not reproduced here — a wrong citation list is worse than none, because it propagates into other people's papers. Do not publish analyses of this dataset until it is corrected.
>
> Reported by `E002` — see [`VALIDATION.md`](VALIDATION.md) for what that means and how to fix it.

## Getting the data

Everything below is under `s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_017976375.1/`. The bucket is public and needs no credentials, but the AWS CLI needs `--no-sign-request` or it will try to sign the request and fail.

|  | Size |  |
|---|---:|---|
| [`raw.vcf.gz`][vcfs/raw.vcf.gz] | 159.7 GiB | unfiltered joint-genotyped calls |
| [`raw.vcf.gz.tbi`][vcfs/raw.vcf.gz.tbi] | 1.1 MiB | index for the above |
| [`filtered.vcf.gz`][vcfs/filtered.vcf.gz] | 249.5 GiB | filtered calls |
| [`filtered.vcf.gz.tbi`][vcfs/filtered.vcf.gz.tbi] | 1.1 MiB | index for the above |
| [`callable_sites.bed`][callable_sites/callable_sites.bed] | 1.3 MiB | the callable-sites mask |
| [`qc_dashboard.html`][qc/qc_dashboard.html] | 21.8 MiB | the snpArcher QC report — opens in a browser |
| `bams/` | 2754.6 GiB | 646 objects — alignments and indexes |

```bash
# one region of the VCF, without downloading the whole thing
bcftools view -r <chr>:<start>-<end> \
  https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_017976375.1/vcfs/raw.vcf.gz

# the alignments — check the size above first
aws s3 sync --no-sign-request \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_017976375.1/bams/ ./bams/

# the masks and QC tables, without the zarr stores
aws s3 sync --no-sign-request --exclude '*.zarr/*' \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_017976375.1/callable_sites/ ./callable_sites/
```

> [!WARNING]
> `callable_sites/` also holds `callable_loci.zarr/`, `depths/`, `depths.zarr/` and `genmap_index/` — zarr stores of thousands of small objects each.
>
> Nothing here lists or sizes them, so the sizes above are a lower bound, and a recursive `sync` without the `--exclude` above will pull all of them.

<details>
<summary>The other 10 published files</summary>

|  | Size |
|---|---:|
| [`qc/qc_report.tsv`][qc/qc_report.tsv] | 29.6 KiB |
| [`qc/individuals.idepth`][qc/individuals.idepth] | 9.8 KiB |
| [`qc/individuals.het`][qc/individuals.het] | 16.1 KiB |
| [`qc/individuals.imiss`][qc/individuals.imiss] | 13.6 KiB |
| [`qc/individuals.samps.txt`][qc/individuals.samps.txt] | 4.1 KiB |
| [`qc/contig_map.tsv`][qc/contig_map.tsv] | 5.3 KiB |
| [`callable_sites/coverage.bed`][callable_sites/coverage.bed] | 0 B |
| [`callable_sites/mappability.bed`][callable_sites/mappability.bed] | 1.3 MiB |
| [`callable_sites/mappability.bedgraph`][callable_sites/mappability.bedgraph] | 147.8 MiB |
| [`callable_sites/coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv] | 62 B |

</details>

[callable_sites/callable_sites.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_017976375.1/callable_sites/callable_sites.bed
[callable_sites/coverage.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_017976375.1/callable_sites/coverage.bed
[callable_sites/coverage_thresholds.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_017976375.1/callable_sites/coverage_thresholds.tsv
[callable_sites/mappability.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_017976375.1/callable_sites/mappability.bed
[callable_sites/mappability.bedgraph]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_017976375.1/callable_sites/mappability.bedgraph
[qc/contig_map.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_017976375.1/qc/contig_map.tsv
[qc/individuals.het]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_017976375.1/qc/individuals.het
[qc/individuals.idepth]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_017976375.1/qc/individuals.idepth
[qc/individuals.imiss]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_017976375.1/qc/individuals.imiss
[qc/individuals.samps.txt]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_017976375.1/qc/individuals.samps.txt
[qc/qc_dashboard.html]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_017976375.1/qc/qc_dashboard.html
[qc/qc_report.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_017976375.1/qc/qc_report.tsv
[vcfs/filtered.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_017976375.1/vcfs/filtered.vcf.gz
[vcfs/filtered.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_017976375.1/vcfs/filtered.vcf.gz.tbi
[vcfs/raw.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_017976375.1/vcfs/raw.vcf.gz
[vcfs/raw.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_017976375.1/vcfs/raw.vcf.gz.tbi
<!-- congen:end body -->
