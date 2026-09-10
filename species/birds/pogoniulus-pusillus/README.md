<!-- congen:begin body -->
# *Pogoniulus pusillus*

Population-genomic variant calls for 21 *Pogoniulus pusillus* samples ([NCBI taxon 488313](https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=488313)), produced by snpArcher against `GCA_015220805.1` and published on GenomeArk.

> [!TIP]
> **PASS** · validated 2026-09-10 · `GCA_015220805.1`
>
> Metadata agrees with the data published on GenomeArk.
>
> Full report: [`VALIDATION.md`](VALIDATION.md) · check definitions: [`CHECKS.md`](../../../CHECKS.md) · baseline description: [`README.txt`](README.txt)

## Dataset

|  |  |
|---|---|
| Clade | birds |
| Reference assembly | [`GCA_015220805.1`](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_015220805.1/) — bPogPus1.pri, Chromosome |
| Variant caller | gatk 4.6.2.0 |
| Samples | 21, all from SRA runs |
| Variant sites | 58,892,278 |

<details>
<summary>Assembly and pipeline detail</summary>

|  |  |
|---|---|
| Assembly organism | Pogoniulus pusillus |
| Paired RefSeq/GenBank accession | `GCF_015220805.1` |
| NCBI taxon | 488313 |
| Contigs in the VCF | 253 |
| Ploidy / heterozygosity prior | 2 / 0.005 |
| Other tools recorded in the VCF header | bcftools 1.23.1 |

</details>

## Sample QC

Full plots: [the snpArcher QC dashboard][qc/qc_dashboard.html]. Per-sample values are not reproduced here; they are in [`qc_report.tsv`][qc/qc_report.tsv], [`individuals.het`][qc/individuals.het] and [`individuals.imiss`][qc/individuals.imiss].

**Mean depth 17.9×** (IQR 15.7–20.4, range 12.0× `SAMN47555421` to 55.9× `SAMN15098508`), over mapped reads.

| mean depth | samples |  |
|---|---:|---|
| 10–15× | 4 | ███████████ |
| 15–20× | 11 | ██████████████████████████████ |
| 20–30× | 4 | ███████████ |
| 30–40× | 1 | ███ |
| 40–60× | 1 | ███ |

Across the cohort:

|  |  |
|---|---|
| Cohort mean coverage | 19.4× |
| Reads mapped | median 98.7% (range 93.2–99.5%) |
| Duplicates | median 15.2% (range 11.5–28.6%) |
| Properly paired | median 92.8% (range 87.2–97.2%) |
| Missingness F_MISS | median 0.050 (range 0.025–0.209) |
| Inbreeding coefficient F | median 0.499 (range 0.253–0.723) |

The callable-sites mask was built over depths 9–39× ([`coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv]). That is a site-level threshold for the mask, not a per-sample cutoff: samples outside it are neither excluded nor flagged here.

## References

*Not generated yet.* This section will list the BioProjects that contributed reads to this dataset, with a citation for each. Until then, the contributing BioProjects are recorded in [`README.txt`](README.txt) — please cite them when you use this dataset.

## Getting the data

Everything below is under `s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_015220805.1/`. The bucket is public and needs no credentials, but the AWS CLI needs `--no-sign-request` or it will try to sign the request and fail.

|  | Size |  |
|---|---:|---|
| [`raw.vcf.gz`][vcfs/raw.vcf.gz] | 9.2 GiB | unfiltered joint-genotyped calls |
| [`raw.vcf.gz.tbi`][vcfs/raw.vcf.gz.tbi] | 1.1 MiB | index for the above |
| [`callable_sites.bed`][callable_sites/callable_sites.bed] | 5.7 MiB | the callable-sites mask |
| [`qc_dashboard.html`][qc/qc_dashboard.html] | 8.6 MiB | the snpArcher QC report — opens in a browser |
| `bams/` | 249.8 GiB | 42 objects — alignments and indexes |

This run did not produce `filtered.vcf.gz`.

```bash
# one region of the VCF, without downloading the whole thing
bcftools view -r <chr>:<start>-<end> \
  https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_015220805.1/vcfs/raw.vcf.gz

# the alignments — check the size above first
aws s3 sync --no-sign-request \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_015220805.1/bams/ ./bams/

# the masks and QC tables, without the zarr stores
aws s3 sync --no-sign-request --exclude '*.zarr/*' \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_015220805.1/callable_sites/ ./callable_sites/
```

> [!WARNING]
> `callable_sites/` also holds `callable_loci.zarr/`, `depths/`, `depths.zarr/` and `genmap_index/` — zarr stores of thousands of small objects each.
>
> Nothing here lists or sizes them, so the sizes above are a lower bound, and a recursive `sync` without the `--exclude` above will pull all of them.

<details>
<summary>The other 10 published files</summary>

|  | Size |
|---|---:|
| [`qc/qc_report.tsv`][qc/qc_report.tsv] | 2.1 KiB |
| [`qc/individuals.idepth`][qc/individuals.idepth] | 653 B |
| [`qc/individuals.het`][qc/individuals.het] | 1.1 KiB |
| [`qc/individuals.imiss`][qc/individuals.imiss] | 928 B |
| [`qc/individuals.samps.txt`][qc/individuals.samps.txt] | 273 B |
| [`qc/contig_map.tsv`][qc/contig_map.tsv] | 9.2 KiB |
| [`callable_sites/coverage.bed`][callable_sites/coverage.bed] | 38.8 MiB |
| [`callable_sites/mappability.bed`][callable_sites/mappability.bed] | 4.8 MiB |
| [`callable_sites/mappability.bedgraph`][callable_sites/mappability.bedgraph] | 1.2 GiB |
| [`callable_sites/coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv] | 62 B |

</details>

[callable_sites/callable_sites.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_015220805.1/callable_sites/callable_sites.bed
[callable_sites/coverage.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_015220805.1/callable_sites/coverage.bed
[callable_sites/coverage_thresholds.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_015220805.1/callable_sites/coverage_thresholds.tsv
[callable_sites/mappability.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_015220805.1/callable_sites/mappability.bed
[callable_sites/mappability.bedgraph]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_015220805.1/callable_sites/mappability.bedgraph
[qc/contig_map.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_015220805.1/qc/contig_map.tsv
[qc/individuals.het]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_015220805.1/qc/individuals.het
[qc/individuals.idepth]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_015220805.1/qc/individuals.idepth
[qc/individuals.imiss]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_015220805.1/qc/individuals.imiss
[qc/individuals.samps.txt]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_015220805.1/qc/individuals.samps.txt
[qc/qc_dashboard.html]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_015220805.1/qc/qc_dashboard.html
[qc/qc_report.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_015220805.1/qc/qc_report.tsv
[vcfs/raw.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_015220805.1/vcfs/raw.vcf.gz
[vcfs/raw.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_015220805.1/vcfs/raw.vcf.gz.tbi
<!-- congen:end body -->
