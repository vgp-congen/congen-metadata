<!-- congen:begin body -->
# *Hyperoodon ampullatus*

Population-genomic variant calls for 35 *Hyperoodon ampullatus* samples ([NCBI taxon 48744](https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=48744)), produced by snpArcher against `GCA_949752795.1` and published on GenomeArk.

> [!TIP]
> **PASS** · validated 2026-09-10 · `GCA_949752795.1`
>
> Metadata agrees with the data published on GenomeArk.
>
> Full report: [`VALIDATION.md`](VALIDATION.md) · check definitions: [`CHECKS.md`](../../../CHECKS.md) · baseline description: [`README.txt`](README.txt)

## Dataset

|  |  |
|---|---|
| Clade | mammals |
| Reference assembly | [`GCA_949752795.1`](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_949752795.1/) — mHypAmp2.1, Chromosome |
| Variant caller | gatk 4.6.2.0 |
| Samples | 35, all from SRA runs |
| Variant sites | 17,554,311 |

<details>
<summary>Assembly and pipeline detail</summary>

|  |  |
|---|---|
| Assembly organism | Hyperoodon ampullatus |
| Paired RefSeq/GenBank accession | `GCF_949752795.1` |
| NCBI taxon | 48744 |
| Contigs in the VCF | 757 |
| Ploidy / heterozygosity prior | 2 / 0.005 |
| Other tools recorded in the VCF header | bcftools 1.23.1 |

</details>

## Sample QC

Full plots: [the snpArcher QC dashboard][qc/qc_dashboard.html]. Per-sample values are not reproduced here; they are in [`qc_report.tsv`][qc/qc_report.tsv], [`individuals.het`][qc/individuals.het] and [`individuals.imiss`][qc/individuals.imiss].

**Mean depth 10.2×** (IQR 8.7–13.2, range 2.4× `SAMN26138172` to 28.6× `SAMN26263697`), over mapped reads.

| mean depth | samples |  |
|---|---:|---|
| 0–5× | 1 | ██ |
| 5–10× | 14 | ██████████████████████████████ |
| 10–15× | 14 | ██████████████████████████████ |
| 15–20× | 5 | ███████████ |
| 20–30× | 1 | ██ |

Across the cohort:

|  |  |
|---|---|
| Cohort mean coverage | 9.7× |
| Reads mapped | median 99.8% (range 99.2–99.9%) |
| Duplicates | median 22.6% (range 9.8–31.9%) |
| Properly paired | median 98.0% (range 86.7–99.0%) |
| Missingness F_MISS | median 0.177 (range 0.124–0.400) |
| Inbreeding coefficient F | median 0.081 (range -0.121–0.591) |

The callable-sites mask was built over depths 4–20× ([`coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv]). That is a site-level threshold for the mask, not a per-sample cutoff: samples outside it are neither excluded nor flagged here.

## References

*Not generated yet.* This section will list the BioProjects that contributed reads to this dataset, with a citation for each. Until then, the contributing BioProjects are recorded in [`README.txt`](README.txt) — please cite them when you use this dataset.

## Getting the data

Everything below is under `s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_949752795.1/`. The bucket is public and needs no credentials, but the AWS CLI needs `--no-sign-request` or it will try to sign the request and fail.

|  | Size |  |
|---|---:|---|
| [`raw.vcf.gz`][vcfs/raw.vcf.gz] | 3.5 GiB | unfiltered joint-genotyped calls |
| [`raw.vcf.gz.tbi`][vcfs/raw.vcf.gz.tbi] | 2.2 MiB | index for the above |
| [`filtered.vcf.gz`][vcfs/filtered.vcf.gz] | 4.8 GiB | filtered calls |
| [`filtered.vcf.gz.tbi`][vcfs/filtered.vcf.gz.tbi] | 2.2 MiB | index for the above |
| [`callable_sites.bed`][callable_sites/callable_sites.bed] | 9.2 MiB | the callable-sites mask |
| [`qc_dashboard.html`][qc/qc_dashboard.html] | 8.9 MiB | the snpArcher QC report — opens in a browser |
| `bams/` | 547.0 GiB | 70 objects — alignments and indexes |

```bash
# one region of the VCF, without downloading the whole thing
bcftools view -r <chr>:<start>-<end> \
  https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949752795.1/vcfs/raw.vcf.gz

# the alignments — check the size above first
aws s3 sync --no-sign-request \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_949752795.1/bams/ ./bams/

# the masks and QC tables, without the zarr stores
aws s3 sync --no-sign-request --exclude '*.zarr/*' \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_949752795.1/callable_sites/ ./callable_sites/
```

> [!WARNING]
> `callable_sites/` also holds `callable_loci.zarr/`, `depths/`, `depths.zarr/` and `genmap_index/` — zarr stores of thousands of small objects each.
>
> Nothing here lists or sizes them, so the sizes above are a lower bound, and a recursive `sync` without the `--exclude` above will pull all of them.

<details>
<summary>The other 10 published files</summary>

|  | Size |
|---|---:|
| [`qc/qc_report.tsv`][qc/qc_report.tsv] | 3.4 KiB |
| [`qc/individuals.idepth`][qc/individuals.idepth] | 1.0 KiB |
| [`qc/individuals.het`][qc/individuals.het] | 1.7 KiB |
| [`qc/individuals.imiss`][qc/individuals.imiss] | 1.4 KiB |
| [`qc/individuals.samps.txt`][qc/individuals.samps.txt] | 455 B |
| [`qc/contig_map.tsv`][qc/contig_map.tsv] | 29.2 KiB |
| [`callable_sites/coverage.bed`][callable_sites/coverage.bed] | 8.5 MiB |
| [`callable_sites/mappability.bed`][callable_sites/mappability.bed] | 8.4 MiB |
| [`callable_sites/mappability.bedgraph`][callable_sites/mappability.bedgraph] | 10.5 GiB |
| [`callable_sites/coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv] | 61 B |

</details>

[callable_sites/callable_sites.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949752795.1/callable_sites/callable_sites.bed
[callable_sites/coverage.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949752795.1/callable_sites/coverage.bed
[callable_sites/coverage_thresholds.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949752795.1/callable_sites/coverage_thresholds.tsv
[callable_sites/mappability.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949752795.1/callable_sites/mappability.bed
[callable_sites/mappability.bedgraph]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949752795.1/callable_sites/mappability.bedgraph
[qc/contig_map.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949752795.1/qc/contig_map.tsv
[qc/individuals.het]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949752795.1/qc/individuals.het
[qc/individuals.idepth]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949752795.1/qc/individuals.idepth
[qc/individuals.imiss]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949752795.1/qc/individuals.imiss
[qc/individuals.samps.txt]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949752795.1/qc/individuals.samps.txt
[qc/qc_dashboard.html]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949752795.1/qc/qc_dashboard.html
[qc/qc_report.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949752795.1/qc/qc_report.tsv
[vcfs/filtered.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949752795.1/vcfs/filtered.vcf.gz
[vcfs/filtered.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949752795.1/vcfs/filtered.vcf.gz.tbi
[vcfs/raw.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949752795.1/vcfs/raw.vcf.gz
[vcfs/raw.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949752795.1/vcfs/raw.vcf.gz.tbi
<!-- congen:end body -->
