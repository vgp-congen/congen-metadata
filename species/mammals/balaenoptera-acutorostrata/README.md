<!-- congen:begin body -->
# *Balaenoptera acutorostrata*

Population-genomic variant calls for 10 *Balaenoptera acutorostrata* samples ([NCBI taxon 9767](https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=9767)), produced by snpArcher against `GCA_949987535.1` and published on GenomeArk.

> [!TIP]
> **PASS** · validated 2026-09-10 · `GCA_949987535.1`
>
> Metadata agrees with the data published on GenomeArk.
>
> Full report: [`VALIDATION.md`](VALIDATION.md) · check definitions: [`CHECKS.md`](../../../CHECKS.md) · baseline description: [`README.txt`](README.txt)

## Dataset

|  |  |
|---|---|
| Clade | mammals |
| Reference assembly | [`GCA_949987535.1`](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_949987535.1/) — mBalAcu1.1, Chromosome |
| Variant caller | gatk 4.6.2.0 |
| Samples | 10 across 13 sheet rows, all from SRA runs |
| Variant sites | 12,657,686 |

<details>
<summary>Assembly and pipeline detail</summary>

|  |  |
|---|---|
| Assembly organism | Balaenoptera acutorostrata |
| Paired RefSeq/GenBank accession | `GCF_949987535.1` |
| NCBI taxon | 9767 |
| Contigs in the VCF | 1375 |
| Ploidy / heterozygosity prior | 2 / 0.005 |
| Other tools recorded in the VCF header | bcftools 1.23.1 |

</details>

## Sample QC

Full plots: [the snpArcher QC dashboard][qc/qc_dashboard.html]. Per-sample values are not reproduced here; they are in [`qc_report.tsv`][qc/qc_report.tsv], [`individuals.het`][qc/individuals.het] and [`individuals.imiss`][qc/individuals.imiss].

**Mean depth 26.3×** (IQR 12.9–29.6, range 10.6× `SAMN03339800` to 41.9× `SAMN37420689`), over mapped reads.

| mean depth | samples |  |
|---|---:|---|
| 10–15× | 4 | ██████████████████████████████ |
| 15–20× | 0 |  |
| 20–30× | 3 | ██████████████████████ |
| 30–40× | 2 | ███████████████ |
| 40–60× | 1 | ████████ |

Across the cohort:

|  |  |
|---|---|
| Cohort mean coverage | 22.4× |
| Reads mapped | median 99.8% (range 99.7–99.9%) |
| Duplicates | median 2.7% (range 0.8–13.4%) |
| Properly paired | median 95.4% (range 73.2–98.9%) |
| Missingness F_MISS | median 0.087 (range 0.037–0.136) |
| Inbreeding coefficient F | median 0.285 (range -0.730–0.337) |

The callable-sites mask was built over depths 11–45× ([`coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv]). That is a site-level threshold for the mask, not a per-sample cutoff: samples outside it are neither excluded nor flagged here.

## References

*Not generated yet.* This section will list the BioProjects that contributed reads to this dataset, with a citation for each. Until then, the contributing BioProjects are recorded in [`README.txt`](README.txt) — please cite them when you use this dataset.

## Getting the data

Everything below is under `s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_949987535.1/`. The bucket is public and needs no credentials, but the AWS CLI needs `--no-sign-request` or it will try to sign the request and fail.

|  | Size |  |
|---|---:|---|
| [`raw.vcf.gz`][vcfs/raw.vcf.gz] | 1.3 GiB | unfiltered joint-genotyped calls |
| [`raw.vcf.gz.tbi`][vcfs/raw.vcf.gz.tbi] | 1.9 MiB | index for the above |
| [`callable_sites.bed`][callable_sites/callable_sites.bed] | 22.6 MiB | the callable-sites mask |
| [`qc_dashboard.html`][qc/qc_dashboard.html] | 8.4 MiB | the snpArcher QC report — opens in a browser |
| `bams/` | 460.5 GiB | 20 objects — alignments and indexes |

This run did not produce `filtered.vcf.gz`.

```bash
# one region of the VCF, without downloading the whole thing
bcftools view -r <chr>:<start>-<end> \
  https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949987535.1/vcfs/raw.vcf.gz

# the alignments — check the size above first
aws s3 sync --no-sign-request \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_949987535.1/bams/ ./bams/

# the masks and QC tables, without the zarr stores
aws s3 sync --no-sign-request --exclude '*.zarr/*' \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_949987535.1/callable_sites/ ./callable_sites/
```

> [!WARNING]
> `callable_sites/` also holds `callable_loci.zarr/`, `depths/`, `depths.zarr/` and `genmap_index/` — zarr stores of thousands of small objects each.
>
> Nothing here lists or sizes them, so the sizes above are a lower bound, and a recursive `sync` without the `--exclude` above will pull all of them.

<details>
<summary>The other 10 published files</summary>

|  | Size |
|---|---:|
| [`qc/qc_report.tsv`][qc/qc_report.tsv] | 1.1 KiB |
| [`qc/individuals.idepth`][qc/individuals.idepth] | 324 B |
| [`qc/individuals.het`][qc/individuals.het] | 510 B |
| [`qc/individuals.imiss`][qc/individuals.imiss] | 459 B |
| [`qc/individuals.samps.txt`][qc/individuals.samps.txt] | 130 B |
| [`qc/contig_map.tsv`][qc/contig_map.tsv] | 53.7 KiB |
| [`callable_sites/coverage.bed`][callable_sites/coverage.bed] | 85.7 MiB |
| [`callable_sites/mappability.bed`][callable_sites/mappability.bed] | 7.3 MiB |
| [`callable_sites/mappability.bedgraph`][callable_sites/mappability.bedgraph] | 10.3 GiB |
| [`callable_sites/coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv] | 63 B |

</details>

[callable_sites/callable_sites.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949987535.1/callable_sites/callable_sites.bed
[callable_sites/coverage.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949987535.1/callable_sites/coverage.bed
[callable_sites/coverage_thresholds.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949987535.1/callable_sites/coverage_thresholds.tsv
[callable_sites/mappability.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949987535.1/callable_sites/mappability.bed
[callable_sites/mappability.bedgraph]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949987535.1/callable_sites/mappability.bedgraph
[qc/contig_map.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949987535.1/qc/contig_map.tsv
[qc/individuals.het]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949987535.1/qc/individuals.het
[qc/individuals.idepth]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949987535.1/qc/individuals.idepth
[qc/individuals.imiss]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949987535.1/qc/individuals.imiss
[qc/individuals.samps.txt]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949987535.1/qc/individuals.samps.txt
[qc/qc_dashboard.html]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949987535.1/qc/qc_dashboard.html
[qc/qc_report.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949987535.1/qc/qc_report.tsv
[vcfs/raw.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949987535.1/vcfs/raw.vcf.gz
[vcfs/raw.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_949987535.1/vcfs/raw.vcf.gz.tbi
<!-- congen:end body -->
