# Validation checks

Every finding in a `VALIDATION.md` carries an ID. This is what they mean.

Generated from congen-metadata-tools 0.1.0; 44 checks.

Severities:

- **error** — a defect: do not rely on this metadata until it is resolved
- **warn** — worth attention, but not disqualifying
- **info** — recorded for reference; no action implied

A check may also be *skipped*, which means its inputs were unavailable —
most often because the species has no published data yet. A skipped check
is not a pass; the report says how many were skipped and why.

## Repository self-consistency

### R001

**error** — config.yaml parses and has the required keys

### R002

**error** — reference.source is an accession, URL or path

### R003

**warn** — reference.name looks like a species name

snpArcher stages the reference as ``results/reference/<name>.fa.gz``.

An accession there means the staged file is named after the accession rather than the species, which is legal but makes the BAM provenance harder to read and breaks the `F006` cross-check.

### R010

**error** — sample_sheet.csv parses with the required columns

Structural problems the sheet loader found.

Covers a missing or unreadable file, absent required columns, rows with too few fields, an empty ``sample_id``, an unrecognized ``input_type``, and a genuinely repeated ``(sample_id, input)`` pair. A blank row is reported here too, but only as a note.

### R013

**error** — srr inputs are SRA run or experiment accessions

### R015

**warn** — inputs are not local filesystem paths

A path on someone's cluster cannot be re-fetched by anyone else.

The run is not reproducible from the sheet alone: whoever repeats it needs the original filesystem. Usually a sign that reads were recovered locally rather than pulled from SRA.

### R016

**warn** — sample_id values are BioSample accessions

### R017

**error** — srr inputs identify exactly one run each

An experiment holding several runs does not say which reads were used.

Error when it expands to more than one run — that is a genuine ambiguity about what was analysed. Informational when it expands to exactly one, which is the case for all 42 experiment accessions in `birds/anser-anser`: worth recording as an imprecise way to name a run, not worth blocking on.

### R020

**warn** — README.txt accession matches the config

## Publication on GenomeArk

### G001

**error** — the accession has a prefix on GenomeArk

### G010

**warn** — vcfs/raw.vcf.gz is present

A warning, not an error: an incomplete upload may be mid-flight.

The tool cannot know whether a publication was supposed to have finished, so calling it an error overclaims. It stays a finding because an upload that started and stopped is worth surfacing, and the status block explains the rest.

### G011

**error** — the raw VCF has an index

### G012

**error** — bams/ is not empty

### G013

**error** — every BAM has an index

### G014

**error** — no zero-byte data objects

### G017

**warn** — the repo and GenomeArk READMEs agree

The repo copy and the published copy have drifted apart.

The structural analogue of `S006` for the sample sheet: two copies of one document, and one of them is behind. Almost always a sync gap rather than a disagreement — across the corpus, eleven species have a README on GenomeArk that was never copied back, and none have differing content — so the fix is usually mechanical.

Silent when neither side has one; that is `G018`.

### G018

**warn** — a README.txt exists somewhere

Nobody has documented this dataset, on either side.

The README is where the contributing bioprojects are recorded, so without one there is nothing telling a user what to cite. Distinct from `G017`: that one says the two copies disagree, this one says there is no copy to disagree with, which `G017` would read as being trivially in sync.

Fires for unpublished species too. Writing a README does not need the data to exist — the bioprojects come from the sample sheet — so waiting on a run is not a reason to be undocumented.

## Sample identity

### S001

**error** — sheet samples == VCF samples

The sheet and the published VCF must name the same biosamples.

A disagreement means one of them is stale, and which is not derivable from the artifacts — see the note above.

### S002

**error** — sheet samples == published BAMs

Only meaningful once the publication is complete.

During a partial upload the BAM set is by definition not final, so comparing it to the sheet measures how far the upload got rather than whether the metadata is right. `grus-americana` — 57 samples in the sheet, 42 BAMs published, no VCF — is exactly that case, and this check reported it as a metadata mismatch until the status model existed to distinguish the two.

### S003

**error** — VCF samples == published BAMs

### S004

**error** — each BAM's @RG SM matches its filename

### S005

**warn** — qc/individuals.samps.txt agrees with the VCF

### S006

**warn** — the published sample sheet matches the repo copy

Catches upload drift — but note it cannot catch a shared error.

For `anser-albifrons` the published copy is byte-identical to the repo sheet and both disagree with the VCF, which is exactly why this is a warning and S001 is the error.

### S007

**warn** — @RG LB agrees with library_id where the sheet declares it

## Reference identity and canonicality

### F001

**error** — every VCF contig exists in the declared assembly

### F002

**error** — VCF contig lengths match the assembly

The real wrong-genome detector.

A different assembly of the same species reuses naming conventions but not sequence lengths.

### F003

**error** — contig names use one consistent naming scheme

### F004

**error** — BAM @SQ matches the VCF contigs

### F005

**error** — all BAMs share one @SQ list

Note the sample size: by default only a few BAMs are read.

A clean result therefore means "the BAMs examined agree", which the finding says explicitly rather than implying it covered all of them.

### F006

**warn** — the BAM's bwa command line names reference.name

### F008

**warn** — qc/contig_map.tsv agrees with the VCF contigs

### F009

**warn** — assembly sequences absent from the VCF

The permissive direction of the name comparison.

Warns above the threshold, informs below it, so the ordinary case of a few dropped short scaffolds stays quiet without hiding a reference that is missing a real fraction of the genome.

### F011

**warn** — no whole assembled molecule is absent from the VCF

More sensitive than F009, at any fraction.

A missing unplaced scaffold is unremarkable; a missing chromosome is not, however small it is.

### F020

**error** — reference.source is the VGP main-haplotype assembly

The config names an assembly that is not the VGP reference.

Not a GCA/GCF namespace variant of the right one — see `F021` for that — but a different assembly altogether, so any run against it used the wrong genome.

### F021

**error** — reference.source uses the canonical accession form

The GCA/GCF counterpart of the right assembly is still wrong.

An error rather than a warning because the VGP list is an authority and the fix is mechanical: normalize to the listed accession. Before the list existed this could only be a warning.

### F022

**error** — the species is in the VGP reference list

Every congen species is by definition a VGP species.

An error rather than a warning, even though it fires on nothing today: all 79 species resolve. If it ever fires, either the list is stale or the species does not belong in the corpus, and both want attention rather than a line in the warnings.

### F023

**warn** — the VGP species name agrees with the directory and reference.name

### F024

**warn** — the assembly's NCBI taxid matches the VGP list

The VGP list's `QID` column holds NCBI taxonomy IDs, not Wikidata QIDs.

## Config against recorded provenance

### P001

**warn** — variant_calling.ploidy matches the recorded --sample-ploidy

### P002

**warn** — gatk.het_prior matches the recorded --heterozygosity

### P003

**warn** — variant_calling.tool matches the caller recorded in the VCF

Only reports when a *different* caller is positively identified.

Absence of evidence is not evidence: a header this code does not recognize produces nothing rather than a guess. Note also that every GATK-called VCF here carries `##bcftools_concatCommand`, because snpArcher merges its per-interval VCFs with bcftools — which is why caller detection looks for `bcftools_call` specifically and not for bcftools in general.

## External accessions (NCBI SRA)

### E001

**error** — each srr input belongs to the biosample the sheet claims

### E002

**warn** — every run's bioproject is listed in README.txt

Runs are drawn from a bioproject the README does not cite.

Usually means a README lists the assembly's BioProject rather than the one holding the reads, or that samples were added from a project nobody recorded. Affects citation, not data correctness.

### E003

**warn** — every README.txt bioproject contributes runs

The README cites a bioproject that contributes no reads.

Often the RefSeq genome-assembly BioProject, which by definition holds no runs; the reads live under a separate raw-reads project. Frequently appears alongside `E002`, as two views of one mistake.

<!-- generated by congen-metadata-tools; do not edit by hand -->
