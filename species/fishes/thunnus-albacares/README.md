<!-- congen:begin body -->
# *Thunnus albacares*

Population-genomic variant calls for 11 *Thunnus albacares* samples ([NCBI taxon 8236](https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=8236)), produced by snpArcher against `GCA_914725855.1` and published on GenomeArk.

> [!TIP]
> **PASS** · validated 2026-09-10 · `GCA_914725855.1`
>
> Metadata agrees with the data published on GenomeArk.
>
> Full report: [`VALIDATION.md`](VALIDATION.md) · check definitions: [`CHECKS.md`](../../../CHECKS.md) · baseline description: [`README.txt`](README.txt)

## Dataset

|  |  |
|---|---|
| Clade | fishes |
| Reference assembly | [`GCA_914725855.1`](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_914725855.1/) — fThuAlb1.1, Chromosome |
| Variant caller | gatk 4.6.2.0 |
| Samples | 11 across 31 sheet rows, all from SRA runs |
| Variant sites | 25,562,549 |

<details>
<summary>Assembly and pipeline detail</summary>

|  |  |
|---|---|
| Assembly organism | Thunnus albacares |
| Paired RefSeq/GenBank accession | `GCF_914725855.1` |
| NCBI taxon | 8236 |
| Contigs in the VCF | 69 |
| Ploidy / heterozygosity prior | 2 / 0.005 |
| Other tools recorded in the VCF header | bcftools 1.23.1 |

</details>

## Sample QC

Full plots: [the snpArcher QC dashboard][qc/qc_dashboard.html]. Per-sample values are not reproduced here; they are in [`qc_report.tsv`][qc/qc_report.tsv], [`individuals.het`][qc/individuals.het] and [`individuals.imiss`][qc/individuals.imiss].

**Mean depth 14.8×** (IQR 12.4–19.4, range 10.7× `SAMEA4032484` to 134.7× `SAMN36466073`), over mapped reads.

| mean depth | samples |  |
|---|---:|---|
| 10–15× | 6 | ██████████████████████████████ |
| 15–20× | 3 | ███████████████ |
| 20–30× | 0 |  |
| 30–40× | 0 |  |
| 40–60× | 0 |  |
| 60–100× | 0 |  |
| 100×+ | 2 | ██████████ |

Across the cohort:

|  |  |
|---|---|
| Cohort mean coverage | 30.0× |
| Reads mapped | median 99.1% (range 93.2–99.8%) |
| Duplicates | median 1.2% (range 0.6–17.7%) |
| Properly paired | median 86.8% (range 72.5–97.2%) |
| Missingness F_MISS | median 0.069 (range 0.018–0.182) |
| Inbreeding coefficient F | median 0.079 (range -0.070–0.179) |

The callable-sites mask was built over depths 14–60× ([`coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv]). That is a site-level threshold for the mask, not a per-sample cutoff: samples outside it are neither excluded nor flagged here.

## References

*Not generated yet.* This section will list the BioProjects that contributed reads to this dataset, with a citation for each. Until then, the contributing BioProjects are recorded in [`README.txt`](README.txt) — please cite them when you use this dataset.

## Getting the data

Everything below is under `s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_914725855.1/`. The bucket is public and needs no credentials, but the AWS CLI needs `--no-sign-request` or it will try to sign the request and fail.

|  | Size |  |
|---|---:|---|
| [`raw.vcf.gz`][vcfs/raw.vcf.gz] | 2.9 GiB | unfiltered joint-genotyped calls |
| [`raw.vcf.gz.tbi`][vcfs/raw.vcf.gz.tbi] | 668.2 KiB | index for the above |
| [`callable_sites.bed`][callable_sites/callable_sites.bed] | 2.4 MiB | the callable-sites mask |
| [`qc_dashboard.html`][qc/qc_dashboard.html] | 8.4 MiB | the snpArcher QC report — opens in a browser |
| `bams/` | 125.2 GiB | 22 objects — alignments and indexes |

This run did not produce `filtered.vcf.gz`.

```bash
# one region of the VCF, without downloading the whole thing
bcftools view -r <chr>:<start>-<end> \
  https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_914725855.1/vcfs/raw.vcf.gz

# the alignments — check the size above first
aws s3 sync --no-sign-request \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_914725855.1/bams/ ./bams/

# the masks and QC tables, without the zarr stores
aws s3 sync --no-sign-request --exclude '*.zarr/*' \
  s3://genomeark/downstream_analyses/conservation_genomics/variant_calling/GCA_914725855.1/callable_sites/ ./callable_sites/
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
| [`qc/individuals.idepth`][qc/individuals.idepth] | 352 B |
| [`qc/individuals.het`][qc/individuals.het] | 583 B |
| [`qc/individuals.imiss`][qc/individuals.imiss] | 503 B |
| [`qc/individuals.samps.txt`][qc/individuals.samps.txt] | 143 B |
| [`qc/contig_map.tsv`][qc/contig_map.tsv] | 2.3 KiB |
| [`callable_sites/coverage.bed`][callable_sites/coverage.bed] | 11.7 MiB |
| [`callable_sites/mappability.bed`][callable_sites/mappability.bed] | 1.8 MiB |
| [`callable_sites/mappability.bedgraph`][callable_sites/mappability.bedgraph] | 186.7 MiB |
| [`callable_sites/coverage_thresholds.tsv`][callable_sites/coverage_thresholds.tsv] | 63 B |

</details>

[callable_sites/callable_sites.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_914725855.1/callable_sites/callable_sites.bed
[callable_sites/coverage.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_914725855.1/callable_sites/coverage.bed
[callable_sites/coverage_thresholds.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_914725855.1/callable_sites/coverage_thresholds.tsv
[callable_sites/mappability.bed]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_914725855.1/callable_sites/mappability.bed
[callable_sites/mappability.bedgraph]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_914725855.1/callable_sites/mappability.bedgraph
[qc/contig_map.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_914725855.1/qc/contig_map.tsv
[qc/individuals.het]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_914725855.1/qc/individuals.het
[qc/individuals.idepth]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_914725855.1/qc/individuals.idepth
[qc/individuals.imiss]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_914725855.1/qc/individuals.imiss
[qc/individuals.samps.txt]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_914725855.1/qc/individuals.samps.txt
[qc/qc_dashboard.html]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_914725855.1/qc/qc_dashboard.html
[qc/qc_report.tsv]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_914725855.1/qc/qc_report.tsv
[vcfs/raw.vcf.gz]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_914725855.1/vcfs/raw.vcf.gz
[vcfs/raw.vcf.gz.tbi]: https://genomeark.s3.amazonaws.com/downstream_analyses/conservation_genomics/variant_calling/GCA_914725855.1/vcfs/raw.vcf.gz.tbi
<!-- congen:end body -->
