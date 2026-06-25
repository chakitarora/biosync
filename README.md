# Bio.Sync — Biological Identifier Harmonization

A browser-based tool for resolving, converting, validating, and comparing biological identifiers across major databases. Paste any identifier — UniProt accession, Ensembl ID, RefSeq, HGNC, GO term, Reactome pathway, ChEMBL target, dbSNP rsID, ClinVar variant, gene symbol — and instantly get all equivalent identifiers, aliases, species, provenance, and source links.

**Live tool:** `https://chakitarora.github.io/biosync`

No account. No install. Works in any modern browser. Results cached locally for 7 days. Paste anywhere on the page to auto-fill the single lookup. Share any query as a URL parameter: `?q=P07550`.

---

## What problem this solves

Biological databases use incompatible identifier systems. The same gene may appear as `ADRB2`, `154`, `ENSG00000169252`, `NM_000024`, `P07550`, or `HGNC:286` depending on the database and publication year. Researchers encounter identifiers from papers, pipelines, and collaborators and spend time manually cross-referencing them — one at a time, across multiple browser tabs.

Bio.Sync handles the full identifier workflow in one place:
- **What is this identifier?** → Single lookup
- **What are all identifiers for this list?** → Batch
- **I have a name, not an ID** → Name search
- **Are these identifiers still current?** → Validate
- **What is the mouse ortholog of this human gene?** → Orthologs
- **Do these two gene lists from different sources overlap?** → Resolve & Reconcile

---

## Supported identifier types

| Identifier | Example | Database | Entity |
|---|---|---|---|
| UniProt accession | `P07550` | UniProt | Protein |
| Ensembl Gene | `ENSG00000169252` | Ensembl | Gene |
| Ensembl Transcript | `ENST00000359597` | Ensembl | Transcript |
| Ensembl Protein | `ENSP00000354234` | Ensembl | Protein |
| Ensembl Gene (Mouse) | `ENSMUSG00000022592` | Ensembl | Gene |
| Ensembl Transcript (Mouse) | `ENSMUST00000023016` | Ensembl | Transcript |
| RefSeq mRNA | `NM_000024` | RefSeq | Transcript |
| RefSeq ncRNA | `NR_002715` | RefSeq | Transcript |
| RefSeq Protein | `NP_000015` | RefSeq | Protein |
| RefSeq Predicted mRNA | `XM_017021966` | RefSeq | Transcript |
| RefSeq Predicted Protein | `XP_016877455` | RefSeq | Protein |
| RefSeq Genomic | `NG_007524` | RefSeq | Gene |
| HGNC ID | `HGNC:286` | HGNC | Gene |
| GO term | `GO:0007165` | Gene Ontology | Function/Process/Component |
| Reactome pathway | `R-HSA-162582` | Reactome | Pathway |
| ChEMBL ID | `CHEMBL210` | ChEMBL | Target |
| InterPro | `IPR000276` | InterPro | Domain/Family |
| Pfam | `PF00001` | Pfam | Domain |
| dbSNP rsID | `rs7412` | NCBI dbSNP | Variant |
| ClinVar ID | `VCV000017112` | ClinVar | Variant |
| miRBase | `MI0000060` | miRBase | miRNA |
| LRG identifier | `LRG_292` | LRG | Gene |
| UniParc | `UPI0000000001` | UniParc | Protein |
| NCBI Gene ID | `154` | NCBI Gene | Gene |
| PDB ID | `2RH1` | PDB | Structure |
| Gene symbol | `ADRB2` | Multiple | Gene |

**All input is case-insensitive.** `p07550`, `ensg00000169252`, `go:0007165`, `adrb2` all resolve correctly.

**Versioned identifiers** (`NM_000024.6`, `ENSG00000169252.15`) are accepted — the version suffix is stripped and an amber notice shows which version was submitted.

**Global paste:** paste anywhere on the page (not just inside the input box) to auto-fill the single lookup and resolve immediately.

**Multi-identifier redirect:** pasting a comma-separated list into the single input box automatically switches to Batch mode with the identifiers pre-filled.

---

## The seven modes

---

### 1 — Single lookup

**When to use:** you have one identifier and want to know what it is, what it maps to across databases, and what its aliases and function are.

Paste any identifier into the search box. Detection happens locally in milliseconds — no network call needed for the identification step. Results show:

**Detected strip** — a horizontal rail showing: input value, identifier type, database, entity type, and confidence level. Confidence is shown as a signal dot: filled circle (high — unambiguous format), half-ring (medium — format could match multiple types), empty circle (low — gene symbol, matched last).

**Version note** — amber bar when a version suffix was stripped, showing what was submitted and what was resolved.

**Known complexity warning** — red bar for 25 identifiers with documented ambiguity. Examples:
- GNAS: encodes 5+ distinct proteins from one locus (Gsα, XLαs, NESP55, A/B, Alex)
- AKT: three distinct isoforms (AKT1, AKT2, AKT3) — specify which
- VEGF: not an approved HGNC symbol — use VEGFA, VEGFB, or VEGFC
- H1-0: renamed from H1F0 in 2022; pre-2022 datasets use the old symbol
- ERK: not approved — use MAPK1 (ERK2) or MAPK3 (ERK1)
- TNF/LTA: TNF-α is TNF (P01375); TNF-β is LTA (P01374) — different genes
- NOS: three distinct enzymes (NOS1/NOS2/NOS3 — neuronal/inducible/endothelial)

**Primary name** — the canonical gene symbol or protein/term name, displayed in Fraunces serif.

**Cross-database mappings** — a timetable-style table of rows, each showing:
- Database label (navy)
- Identifier (with link to source record)
- Provenance badge: `curated` (green, manually verified) or `auto` (grey, computationally predicted)
- Copy button to copy the identifier to clipboard

**Isoform info** — for Ensembl transcript IDs: which protein the transcript encodes, protein length in amino acids, genomic coordinates, and strand.

**Aliases and synonyms** — up to 12 alternative names.

**Function** — brief description for UniProt entries (italic serif).

**Variant info** — for dbSNP rsIDs: variant class, gene context, clinical significance. For ClinVar: title, clinical significance, review status, dbSNP cross-reference.

**Cite button** — copies two formatted strings:
- *Data citation* (for methods section): `ADRB2 (ENSG00000169252, accessed via MyGene.info 2024-06-25).`
- *Database citation* (for bibliography): the standard journal reference for the source database.

**↗ Share URL** — copies the current query as a shareable URL (`?q=P07550`).

**Canonical UniProt resolution:** for gene symbols, NCBI Gene IDs, HGNC IDs, and Ensembl Gene IDs, Bio.Sync runs a secondary UniProt search (`reviewed:true`) to retrieve the canonical Swiss-Prot entry — ensuring correct results for complex loci like GNAS where MyGene.info may return the wrong isoform.

**Free-text fallback:** if an identifier is not recognised by any pattern, Bio.Sync automatically runs a MyGene.info free-text search and shows candidate matches before returning an error.

**Examples:**
```
P07550          → ADRB2, Beta-2 adrenergic receptor, Human
                  UniProt (canonical): P07550 [curated]
                  NCBI Gene: 154 [curated]
                  Ensembl Gene: ENSG00000169252 [curated]
                  RefSeq mRNA: NM_000024 [curated]
                  AlphaFold: P07550 [auto]

GO:0007165      → signal transduction (Biological Process)
                  AmiGO link, QuickGO link, 3 synonyms

R-HSA-162582    → Signal Transduction (Homo sapiens)
                  Sub-pathways listed

rs7412          → rs7412, APOE variant
                  dbSNP link, gene context (APOE), variant class: snv
```

---

### 2 — Batch

**When to use:** you have a list of identifiers — from a paper, a pipeline output, a collaborator — and need to resolve them all.

Paste up to **500 identifiers**, one per line (or comma/tab-separated). Any mix of identifier formats is accepted. Duplicates are removed case-insensitively before processing.

**Species filter** — when selected, constrains gene symbol lookups to a specific organism. Does not affect identifier-based lookups (UniProt, Ensembl, RefSeq etc. resolve to whatever species they encode).

**Convert format** — when a format is selected from the dropdown (e.g. `Convert → Ensembl Gene ID`), the result table collapses to three columns: Input | Ensembl Gene ID | Name. This is the bulk conversion workflow: paste 300 gene symbols from a paper, select Ensembl Gene ID, export the two-column TSV for your pipeline. Identifiers with no mapping to the target format are listed separately.

Results build incrementally as each identifier resolves (~6 per second). The result table shows: input ID, detected type, name, species, top mapping, source database.

**Two export buttons:**
- **Export TSV** — full table (all mappings, aliases, provenance, TaxID) or converted format table depending on the mode
- **Export failed IDs** — plain text list of unresolved identifiers and those with no target format mapping

**Workflow example — converting a gene list to Ensembl IDs:**
```r
# In Seurat — get top markers
markers <- FindAllMarkers(seurat_obj, only.pos = TRUE)
top20 <- markers %>% group_by(cluster) %>% slice_max(avg_log2FC, n = 20)
# Copy gene names → paste into Batch → select Convert → Ensembl Gene ID → export TSV
```

```python
# In Scanpy
sc.tl.rank_genes_groups(adata, 'leiden', method='wilcoxon')
genes = [g for cl in adata.uns['rank_genes_groups']['names'] for g in cl]
# Copy → paste into Batch → convert → export
```

---

### 3 — Name search

**When to use:** you have a protein or gene name from a paper but no database ID.

Type any free-text description — protein name, gene description, or alias. Returns up to 5 ranked matches across human, mouse, and rat from MyGene.info. Click any result for a full single lookup.

**Examples:**
```
beta-2 adrenergic receptor  →  ADRB2 (Human), Adrb2 (Mouse)
tumor protein p53           →  TP53 (Human), Trp53 (Mouse)
von Willebrand factor       →  VWF (Human)
BRCA1 kinase                →  BRCA1 (Human) and related entries
```

---

### 4 — Validate

**When to use:** you have a list of identifiers — from your own analysis or from a published dataset — and need to confirm they are still current before submitting a manuscript, reusing a dataset, or building a pipeline.

Paste any number of identifiers. Bio.Sync checks each:

- **Ensembl IDs (ENSG/ENST/ENSP):** checked against the current Ensembl release via the POST batch lookup endpoint. Retired IDs are forwarded to the Ensembl archive endpoint, which returns the current replacement ID if one exists.
- **All other types:** validated by attempted resolution via the appropriate API.

**Result statuses:**

| Status | Meaning |
|---|---|
| `valid` | Found in current database release. Current name shown. |
| `retired` | No longer current. Current replacement ID shown with link to Ensembl. |
| `invalid` | Not found. May be a typo, wrong format, or genuinely absent from the database. |
| `unrecognised` | Format not recognised by Bio.Sync pattern matching. |
| `error` | Network or API error during the check. |

Results are sorted problems-first: retired → invalid → unrecognised → valid.

**Two exports:**
- **Export validation report** — full TSV with every identifier, its status, current name/replacement, and species
- **Export retired IDs** — TSV of retired identifiers with their current replacements, ready to use for updating a dataset

**Practical use case:** your 2021 analysis used 500 Ensembl Gene IDs based on GRCh38.p13. Before submitting in 2024, run them all through Validate to find which have been retired and what they should be replaced with.

---

### 5 — Orthologs

**When to use:** you have a human gene and need the equivalent in mouse (or another model organism), or vice versa.

Enter any identifier (gene symbol, UniProt accession, Ensembl Gene ID, NCBI Gene ID). Select the target species. Bio.Sync resolves the input to an Ensembl Gene ID first if needed, then queries Ensembl comparative genomics.

**Supported target species:** Mouse, Rat, Zebrafish, Fruit fly, C. elegans, S. cerevisiae, Chicken, Xenopus, Human (for starting from a non-human gene).

**Output table:** Ensembl ID (linked to Ensembl), species, orthology type, sequence identity.

**Orthology types and what they mean:**

| Type | Meaning | Confidence |
|---|---|---|
| `ortholog_one2one` | Single clear ortholog in both species | Highest |
| `ortholog_one2many` | One human gene → multiple orthologs (gene duplicated in target) | Medium |
| `ortholog_many2one` | Multiple human genes → one ortholog (gene duplicated in human) | Medium |
| `ortholog_many2many` | Complex duplication history in both lineages | Lowest |

**Examples:**
```
ADRB2 → Mouse:   Adrb2 (ENSMUSG00000031489), one2one, 85% identity
TP53  → Mouse:   Trp53 (ENSMUSG00000059552), one2one, 77% identity
BRCA1 → Zebrafish: brca1 (ENSDARG00000039453), one2one, 59% identity
```

---

### 6 — Resolve & Reconcile

**When to use:** you have two gene lists from different sources — one with gene symbols, one with Ensembl IDs — and need to find what they have in common.

The key feature: **both lists are resolved to a common canonical format before comparison**. This means `ADRB2` in List A and `P07550` in List B will correctly match because both resolve to `ENSG00000169252` when Ensembl Gene ID is chosen as the canonical format. Pure string comparison would miss this.

**How to use:**
1. Paste List A in the left box — any mix of identifier formats
2. Paste List B in the right box — any mix of formats
3. Select a canonical format from the dropdown (Ensembl Gene ID is the safest choice for most gene-based analyses)
4. Click **Resolve & Reconcile →**

Two progress bars show resolution of each list. Comparison happens once both are complete.

**Output:**
- Stats row: number of Shared / Only A / Only B / Failed identifiers
- Canonical format badge showing which format was used
- Three scrollable columns:
  - **Shared** — identifiers present in both lists (shown as original ID → canonical ID)
  - **Only A** — identifiers unique to List A
  - **Only B** — identifiers unique to List B
- Click any canonical ID to run a full single lookup
- Failed section listing identifiers that could not be resolved with reasons

**Four exports:**
- Shared list (canonical IDs + original IDs)
- Only A list
- Only B list
- Full TSV (original ID, canonical ID, group A/B, list label A/B)

**Practical example:**
```
List A: your DE analysis output (300 Ensembl Gene IDs)
List B: published pathway gene list (150 gene symbols from a paper)
Canonical format: Ensembl Gene ID

→ Bio.Sync resolves the 150 gene symbols to Ensembl IDs
→ Compares the two resolved sets
→ Shared: 47 genes in both
→ Only A: 253 genes unique to your analysis
→ Only B: 103 genes from the paper not in your results
```

**Important limitation:** if one identifier resolves to multiple canonical IDs (e.g. GNAS → multiple UniProt accessions), only the first canonical mapping is used for comparison. Choosing Ensembl Gene ID or NCBI Gene ID as the canonical format avoids this issue, as these have 1:1 gene-level mappings.

---

### 7 — Session history

Every resolved identifier is logged in a collapsible drawer accessible from the **History** button in the header. Stores the last 20 lookups this session (identifier, primary name, type). Click any entry to re-run that lookup. History is in-memory only and clears when the tab is closed.

---

## URL deep linking

Any query can be shared as a URL:

```
https://chakitarora.github.io/biosync?q=P07550
https://chakitarora.github.io/biosync?q=rs7412
https://chakitarora.github.io/biosync?q=ADRB2,TP53,BRCA1&mode=batch
```

The `?q=` parameter auto-fills and resolves on page load. Multiple comma-separated identifiers with `&mode=batch` open in Batch mode pre-filled. Use the **↗ Share URL** button in the header to copy the current query URL.

---

## Provenance and reliability

Every cross-database mapping shows a provenance badge:

- **`curated`** (green) — manually verified, often with experimental evidence. Derived from UniProt ECO:0000269 evidence codes or from primary database curators.
- **`auto`** (grey) — computationally predicted or automatically propagated. Less reliable for ambiguous or complex loci.

For gene symbol, NCBI Gene, HGNC, and Ensembl Gene lookups, Bio.Sync runs a two-step resolution: MyGene.info for cross-database mappings, then a secondary UniProt search (`gene:{symbol} AND organism_id:{taxId} AND reviewed:true`) to retrieve the canonical reviewed Swiss-Prot entry. This ensures correct results for complex loci where MyGene.info may return the wrong isoform.

---

## Data sources and APIs

All data is fetched in real time from public, free, CORS-enabled APIs:

| API | Used for | Institution |
|---|---|---|
| UniProt REST (`rest.uniprot.org`) | Direct accession resolution, canonical gene lookup | EMBL-EBI |
| MyGene.info (`mygene.info/v3`) | Gene/transcript/RefSeq resolution, cross-db mappings | Scripps / NCBI |
| Ensembl REST (`rest.ensembl.org`) | Transcript→protein isoform, genomic location, batch validation, orthologs, archive | EMBL-EBI |
| Gene Ontology (`api.geneontology.org`) | GO term name, namespace, definition, synonyms | GO Consortium |
| Reactome (`reactome.org/ContentService`) | Pathway name, species, sub-pathway structure | EMBL-EBI / Ontario |
| InterPro / EBI (`www.ebi.ac.uk/interpro`) | Domain/family classification | EMBL-EBI |
| ChEMBL (`www.ebi.ac.uk/chembl`) | Target name, type, UniProt cross-reference | EMBL-EBI |
| NCBI Entrez (`eutils.ncbi.nlm.nih.gov`) | dbSNP rsID resolution, ClinVar ID resolution | NCBI |

Batch mode uses a 150 ms delay between requests (~6/sec) to stay within API rate limits.

---

## Caching

Results are cached in your browser's `localStorage` for 7 days. The cache count is shown in the header. Cached lookups are instant and work without internet. Click **clear cache** in the header to remove all entries.

Nothing is sent to any server. The cache is entirely local to your browser.

---

## Comparison with existing tools

| Feature | Bio.Sync | UniProt Mapping | bioDBnet | MyGene.info | g:Profiler |
|---|---|---|---|---|---|
| Auto-detects identifier type | Yes | No | No | Partial | No |
| Case-insensitive input | Yes | Yes | Partial | No | Yes |
| Versioned identifiers handled | Yes (with note) | Partial | No | No | No |
| 26 supported identifier formats | Yes | Protein focus | Broad | Gene focus | Gene focus |
| dbSNP / ClinVar variants | Yes | No | No | No | No |
| GO term resolution | Yes | No | No | No | Yes |
| Reactome pathway resolution | Yes | No | No | No | Yes |
| InterPro / Pfam domain resolution | Yes | No | No | No | No |
| Transcript → protein isoform | Yes | No | No | No | No |
| Genomic coordinates | Yes | No | No | No | No |
| Canonical UniProt (2-step) | Yes | n/a | No | No | No |
| Mapping provenance badges | Yes | No | No | No | No |
| Known complexity warnings (25) | Yes | No | No | No | No |
| Batch up to 500 identifiers | Yes | Yes (larger) | Yes | Yes (API) | Yes |
| Batch format conversion built-in | Yes | Partial | Yes | No | No |
| Identifier validation (retired/current) | Yes | No | No | No | No |
| Ortholog lookup | Yes | No | No | No | No |
| Name/alias free-text search | Yes | No | Partial | Yes | No |
| Resolve & Reconcile (cross-format) | Yes | No | No | No | No |
| Session history | Yes | No | No | No | No |
| URL deep linking | Yes | No | No | No | No |
| Global paste anywhere | Yes | No | No | No | No |
| Data + database citation copy | Yes | No | No | No | No |
| Species filter in batch | Yes | No | No | Yes (API) | No |
| Failed ID separate export | Yes | No | No | No | No |
| Result caching (7 days, local) | Yes | No | No | No | No |
| Offline after first lookup | Yes | No | No | No | No |
| Single HTML file, no backend | Yes | No | No | No | No |

**Where other tools are stronger than Bio.Sync:**

- **Azimuth, CellTypist, SingleR** — reference-based automated cell type annotation from expression matrices. Bio.Sync does not do this.
- **g:Profiler, Enrichr** — pathway and GO enrichment analysis on gene sets. Bio.Sync resolves individual identifiers only, not gene set enrichment.
- **bioDBnet** — covers metabolomics identifiers, microarray probe IDs, and some database conversions not supported by Bio.Sync.
- **UniProt Mapping** — handles very large batch jobs (thousands of identifiers) more robustly than Bio.Sync's 500-identifier browser limit. Better for large-scale proteomics.
- **MyGene.info Python client** (`pip install mygene`) — better for programmatic large-scale resolution in scripts. Supports 1000+ IDs per query with no manual pasting.
- **Ensembl BioMart** — more complete for multi-attribute queries (e.g. all transcripts for all genes in a chromosome region). Bio.Sync handles one entity at a time.

---

## Known limitations

**Batch and validation limits:** 500 identifiers per batch session. For larger jobs use MyGene.info Python client or UniProt's programmatic API.

**Resolve & Reconcile:** uses the first canonical mapping for each identifier. GNAS has multiple UniProt accessions (it encodes 5+ proteins); Bio.Sync uses the first. Choosing NCBI Gene ID or Ensembl Gene ID as canonical format avoids this.

**Identifier validation:** Ensembl IDs are checked against the current Ensembl release. Non-Ensembl IDs are validated by attempted resolution — a failed resolution may reflect a genuine invalid ID or a transient network error.

**Ortholog lookup:** Ensembl comparative genomics covers major model organisms well. Poorly annotated species, recently described genes, or genes created by very recent genome re-annotation may return no results.

**rsID / ClinVar:** variant data comes from NCBI dbSNP and ClinVar esummary API. Full variant annotation (consequence prediction, population allele frequencies, functional impact) requires VEP or a dedicated variant database.

**Gene symbol ambiguity:** `TP53` exists in human, mouse, rat, and many other species. Bio.Sync returns the human-preferred top result from MyGene.info and flags when other species matches exist.

**Network required:** identifier type detection is local and instant. All resolution requires internet. Results are cached after the first lookup.

**No enrichment analysis:** Bio.Sync is an identifier translator, not an analysis tool. It does not perform GO enrichment, pathway analysis, or statistical testing.

---

## Citing Bio.Sync and data sources

When using identifier mappings in published work, cite the primary database from which the mapping originates:

**UniProt:**
> UniProt Consortium. UniProt: the Universal Protein Knowledgebase in 2023. *Nucleic Acids Research.* 2023;51(D1):D523–D531.

**Ensembl:**
> Martin FJ et al. Ensembl 2023. *Nucleic Acids Research.* 2023;51(D1):D933–D941.

**MyGene.info:**
> Xin J et al. High-performance web services for querying gene and variant annotation. *Genome Biology.* 2016;17:91.

**Gene Ontology:**
> Gene Ontology Consortium. The Gene Ontology knowledgebase in 2023. *Genetics.* 2023;224(1):iyad031.

**Reactome:**
> Milacic M et al. The Reactome Pathway Knowledgebase 2024. *Nucleic Acids Research.* 2024;52(D1):D672–D678.

The **Cite** button on each result card generates both a data citation (for the methods section) and the appropriate database citation (for the bibliography) automatically.

---

## Design

Bio.Sync uses the visual language of mid-century Swiss Federal Railways printed timetables: white paper, typographic grid, colour used only to encode meaning, no ornament. A timetable tells you all the connections from a departure point and which are direct; Bio.Sync does the same for biological identifiers. Primary names are set in Fraunces (a variable optical-size serif); all identifier strings in JetBrains Mono.

---

## License

MIT — free to use, modify, and redistribute with attribution.

---

*Built by [Chakit Arora](https://chakitarora.github.io) · Post-Doctoral Fellow, BIO@SNS, Scuola Normale Superiore, Pisa*
