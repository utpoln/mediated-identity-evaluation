# Mediated-Identity Evaluation Corpus

This fills a gap the dissertation's own Artifact Availability section
explicitly names: *"The mediated-identity evaluation scripts and raw corpus
data (Chapter 3) are not yet packaged as a separate public artifact."* This
repo is the **raw corpus data** half of that sentence — the real data
underlying Chapter 3's Experiment 3 (over-merge safety) and the independent
quantitative comparison against Nextflow/Snakemake/CWL in
[`visflow3-quant-comparison-artifact`](https://github.com/utpoln/visflow3-quant-comparison-artifact)
(dissertation §7.4), which runs this identical corpus through those three
external systems.

**The evaluation *scripts*** — the code that computes Chapter 3's own
syntactic / transitive-closure / canonical-mediation results against this
corpus — are **not** in this repo. That implementation lives in a separate,
not-yet-public codebase tied to its own publication; Chapter 3 reports the
full evaluation protocol and results directly.

## Files

- **`negative_cases.csv`** (116 rows) — over-merge test cases: a `bridge_id`
  (a probe ID, a historical gene alias, or a withdrawn HGNC symbol) that maps
  to multiple genuinely distinct `target_ids`/`target_symbols`. A safe merge
  operator must never conclude these distinct targets are "the same entity."
- **`positive_cases.csv`** (100 rows) — true-match test cases: an `old_id`
  that maps to exactly one current `true_gene_id`/`true_symbol`. A useful
  merge operator should recover these.
- **`metadata.json`** — exact sourcing and sampling for every case: which of
  three real, curated resources it came from, how many candidates existed in
  that resource (`total_found`), and how many were sampled (`sampled`), plus
  the fixed random seed used.

## Provenance

| Case type | Source | Candidates found | Sampled |
|---|---|---|---|
| Cross-hybridizing probe | NCBI GEO GPL570 (Affymetrix HG-U133 Plus 2.0) platform annotation | 427 | 50 |
| Shared historical alias | NCBI `Homo_sapiens.gene_info.gz` Synonyms column | 2,665 | 50 |
| HGNC gene split (withdrawn symbol) | HGNC withdrawn-symbol report | 16 | 16 (all) |

Seed: `20260729`. Full URLs for each source are in `metadata.json`. All three
are public, freely redistributable reference resources (NCBI GEO, NCBI Gene,
HGNC) — nothing here is scraped from a restricted-access or paywalled source,
and the files contain only gene/probe identifiers and symbols, no
individual-level or otherwise sensitive data.

## What this corpus is used for

- **Dissertation Chapter 3** (Mediated Identity): Experiment 3 evaluates three
  merge *semantics* — syntactic exact-match, transitive closure, and
  canonical mediation — against this exact corpus (see above: that
  evaluation's own implementation is not in this repo).
- **[`visflow3-quant-comparison-artifact`](https://github.com/utpoln/visflow3-quant-comparison-artifact)**
  (dissertation §7.4): the same corpus (copied there for self-containment) run
  through real Snakemake, Nextflow, and CWL pipelines, to measure what the two
  join strategies available in those systems (absent a dedicated
  identity-mediation primitive) actually do.

## License

MIT (see `LICENSE`) for this repository's own files. The underlying data is a
sample from public NCBI GEO, NCBI Gene, and HGNC reference resources — see
the Provenance table above and `metadata.json` for exact sources and URLs.
