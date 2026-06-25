# BioSync — Biological Identifier Harmonization

Paste any biological identifier and instantly get: what it is, what database it belongs to, all equivalent identifiers across databases, aliases, species, provenance, and direct links. Or search by protein name when you don't have an ID at all.

**Live tool:** `https://chakitarora.github.io/biosync`

No account. No install. Works in any modern browser. Results cached locally for 7 days. Requires internet for new lookups (queries public APIs in real time).

---

## The gap this fills

Biological databases use incompatible identifier systems. A gene may be known as `ADRB2`, `154`, `ENSG00000169252`, `NM_000024`, `P07550`, or `HGNC:286` — all the same entity, none obviously equivalent. Researchers encounter identifiers from papers, tools, and databases and spend time manually cross-referencing them.

BioSync solves the full lookup workflow in one place: paste the identifier, get all equivalents, see the provenance of each mapping, and export results for downstream use.

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
| NCBI Gene ID | `154` | NCBI Gene | Gene |
| HGNC ID | `HGNC:286` | HGNC | Gene |
| GO term | `GO:0007165` | Gene Ontology | Function/Process/Component |
| Reactome pathway | `R-HSA-162582` | Reactome | Pathway |
| ChEMBL ID | `CHEMBL210` | ChEMBL | Target |
| InterPro | `IPR000276` | InterPro | Domain/Family |
| Pfam | `PF00001` | Pfam | Domain |
| miRBase | `MI0000060` | miRBase | miRNA |
| LRG identifier | `LRG_292` | LRG | Gene |
| UniParc | `UPI0000000001` | UniParc | Protein |
| PDB ID | `2RH1` | PDB | Structure |
| Gene symbol | `ADRB2` | Multiple | Gene |

**All input is case-insensitive.** `p07550`, `ensg00000169252`, `go:0007165`, `adrb2` all work.

---

## The three modes

### Single lookup

Paste one identifier. Identifier type is detected locally in milliseconds before any network call. Results show:

- **Detected type** — identifier format, database, entity type, and confidence (high/medium/low)
- **Primary name** — canonical gene symbol or protein/term name
- **Cross-database mappings** — with provenance badges (`curated` = manually verified, `auto` = computationally predicted) and direct links
- **Aliases and synonyms**
- **Species** and organism
- **Function** — brief description (UniProt entries)
- **Isoform info** — for Ensembl transcripts: which protein it encodes, length in amino acids
- **Genomic location** — chromosome, coordinates, strand (Ensembl entries)
- **Copy button** on each mapping ID

If you paste multiple identifiers into the single box, BioSync detects this and automatically switches to Batch mode.

If the identifier is not recognised by pattern, BioSync runs a free-text fallback search automatically before returning an error.

---

### Batch

Paste up to 100 identifiers — one per line, or comma/tab-separated. Duplicates are removed case-insensitively before processing.

**Species filter** — select Human, Mouse, Rat, Zebrafish, Fly, or C. elegans to constrain gene symbol lookups to a specific organism.

Results build incrementally as each identifier resolves (~6 per second to stay within API rate limits). The output table shows: input ID, detected type, name, species, top mapping, source.

**Two export options:**
- **Export TSV** — full table with all mappings, aliases, provenance flags, and source
- **Export failed IDs** — a plain text file of all identifiers that could not be resolved, ready to paste back into another tool or investigate manually

---

### Name search

Search by protein name, gene description, or alias when you don't have a database ID. Type any free-text description and get matching gene entries across human, mouse, and rat.

Examples:
- `beta-2 adrenergic receptor` → ADRB2 (Human), Adrb2 (Mouse)
- `tumor protein p53` → TP53 (Human), Trp53 (Mouse)
- `BRCA1 kinase` → BRCA1 and related entries

Results are clickable — selecting a result runs a full lookup for that gene.

---

## Identifier detection

Detection is local and instant — no network call needed. All input is uppercased before matching.

**Confidence levels:**

- `high` — unambiguous format: UniProt accession character pattern, Ensembl ENSG/ENST/ENSP prefix, RefSeq NM_/NP_/NR_ prefix, HGNC: prefix, GO: prefix, R- Reactome prefix, ChEMBL prefix, IPR/PF prefix
- `medium` — format could match multiple types: pure integer (NCBI Gene ID vs OMIM), 4-character alphanumeric (PDB)
- `low` — gene symbol: matched only after all specific patterns fail

For gene symbols (low confidence), MyGene.info may return results across multiple species. The top result is shown (human preferred), with other species listed for manual selection.

---

## Mapping provenance

Every cross-database mapping now shows a provenance badge:

- `curated` (green) — mapping is manually curated and experimentally verified
- `auto` (grey) — mapping is computationally predicted or automatically generated

This distinction matters for publication: a curated UniProt → Ensembl mapping is more reliable than an automatic one. Hover the badge for a tooltip explanation.

Provenance data comes from UniProt's evidence codes (ECO:0000269 = experimental evidence used in manual assertion).

---

## Data sources and APIs

All data is fetched in real time from free public APIs:

| API | Used for | Base URL |
|---|---|---|
| UniProt REST | UniProt accession resolution, cross-references, function | `rest.uniprot.org` |
| MyGene.info | Gene/transcript/RefSeq resolution, cross-database mappings | `mygene.info/v3` |
| Ensembl REST | Transcript→protein isoform mapping, genomic location | `rest.ensembl.org` |
| Gene Ontology | GO term name, namespace, definition, synonyms | `api.geneontology.org` |
| Reactome | Pathway name, species, sub-pathway structure | `reactome.org/ContentService` |
| InterPro | Domain/family name and classification | `www.ebi.ac.uk/interpro` |
| ChEMBL | Target name, type, UniProt cross-reference | `www.ebi.ac.uk/chembl` |

All APIs are free, require no authentication, and are CORS-enabled for browser use.

Batch mode uses a 150 ms delay between requests (~6/sec) to remain well within all rate limits.

---

## Caching

Results are cached in `localStorage` for 7 days. Cached lookups are instant and work without internet. The cache count is shown in the header. Click **clear cache** to remove all cached entries.

Cache is stored only in your browser — nothing is sent to any server.

---

## Comparison with existing tools

| Feature | BioSync | UniProt Mapping | bioDBnet | MyGene.info | g:Profiler |
|---|---|---|---|---|---|
| Auto-detects ID type | Yes | No | No | Partial | No |
| Case-insensitive input | Yes | Yes | Partial | No | Yes |
| GO term resolution | Yes | No | No | No | Yes |
| Reactome pathway resolution | Yes | No | No | No | Yes |
| InterPro / Pfam | Yes | No | No | No | No |
| Transcript → protein isoform | Yes | No | No | No | No |
| Genomic location | Yes | No | No | No | No |
| Mapping provenance | Yes | No | No | No | No |
| Name/alias search | Yes | No | Partial | Yes | No |
| Species filter in batch | Yes | No | No | Yes (API) | No |
| Failed ID separate export | Yes | No | No | No | No |
| Result caching | Yes (7d) | No | No | No | No |
| Offline after first lookup | Yes | No | No | No | No |
| Single file, no backend | Yes | No | No | No | No |

**Where other tools are better:**
- **g:Profiler** covers pathway and GO enrichment analysis — BioSync only resolves individual GO/Reactome IDs, not sets
- **bioDBnet** covers a wider range of database-to-database conversion paths including metabolomics and microarray probe IDs
- **UniProt Mapping** handles very large batch jobs (thousands of IDs) better than BioSync's 100-identifier browser limit
- **MyGene.info Python client** (`pip install mygene`) is better for programmatic large-scale work

---

## Known limitations

**Batch limit of 100** — for larger jobs use MyGene.info Python client or UniProt's batch mapping API.

**Gene symbol ambiguity** — `TP53` exists in human, mouse, rat, and others. BioSync returns the top result (human preferred) and flags other species.

**NCBI Gene IDs** — a bare integer like `154` is medium confidence: it matches NCBI Gene ID format but could be another numeric identifier.

**GO and Reactome** — BioSync resolves individual term/pathway IDs. It does not perform enrichment analysis on gene lists.

**Isoform completeness** — Ensembl REST returns transcript metadata but not the full isoform family for a gene. Use Ensembl directly for complete isoform views.

**Network required** — identifier type detection is local and instant, but resolving mappings requires internet. Results are cached after first lookup.

---

## Disclaimer

BioSync is a research convenience tool. All data is fetched in real time from third-party APIs and reflects their current state. Mappings may be incomplete, outdated, or incorrect for deprecated or ambiguous identifiers. Verify critical mappings against primary database records before use in publications. Not affiliated with EMBL-EBI, NCBI, or any data provider.

---

## License

MIT

---

*Built by [Chakit Arora](https://chakitarora.github.io) · Post-Doctoral Fellow, BIO@SNS, Scuola Normale Superiore, Pisa*
