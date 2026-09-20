# Oleg Lvovitch

Distinguished Engineer at Elastic, in London. I work across the ES|QL query engine — the
language, the planner, the execution layer. My focus lately is data federation: letting a
query reach data where it already lives, instead of requiring it to be indexed first.

That starts with files in blob storage — Parquet, CSV, TSV, NDJSON, ORC on S3, GCS and
Azure — and moves outward from there, to data lakes and to whatever else is worth reaching.
The point is to widen what the Elastic ecosystem can answer questions about.

Before Elastic, AWS. Before that, Box. About twenty-five years of building systems that
have to stay correct under load.

## The work

Most of it is in [elastic/elasticsearch](https://github.com/elastic/elasticsearch): the
ES|QL engine itself under `x-pack/plugin/esql`, and the `esql-datasource-*` plugins —
[merged pull requests](https://github.com/elastic/elasticsearch/pulls?q=is%3Apr+author%3Aquackaplop+is%3Amerged).

Six that show the shape of the problems:

- **[#157786](https://github.com/elastic/elasticsearch/pull/157786)** — cached statistics
  were blind to *how* a file had been read, so a warm `COUNT(*)` could serve another
  dataset's number. A cache key that doesn't determine its value is the worst class of
  defect in a query engine: the answer comes back wrong, fast, and reproducible.
- **[#148678](https://github.com/elastic/elasticsearch/pull/148678)** — `@Fixed` evaluator
  parameters made JIT-constant through lightweight runtime codegen, so the JIT can fold
  them instead of reloading them per row.
- **[#150920](https://github.com/elastic/elasticsearch/pull/150920)** — answer warm
  `COUNT`/`MIN`/`MAX` over external files from cached statistics rather than reading the
  data again.
- **[#152473](https://github.com/elastic/elasticsearch/pull/152473)** — user-declared
  schemas for external datasets, so types are stated rather than guessed from a sample.
- **[#153640](https://github.com/elastic/elasticsearch/pull/153640)** — partition pruning
  on Hive-partitioned datasets: a filter on the partition column stops listing the files
  it already excludes.
- **[#153579](https://github.com/elastic/elasticsearch/pull/153579)** — a `SORT` crash and
  wrong answers from partition filters over those same datasets.

A theme runs through most of them. Reading someone else's files at query time means the
engine cannot assume anything: not the schema, not the encoding, not that the connection
survives, not that the bytes are what the header claims. Most of the difficulty is in
being correct about data you did not write.

## Other accounts

[`oleglvovitch`](https://github.com/oleglvovitch) (Box) and
[`oleg-lvovitch-aws`](https://github.com/oleg-lvovitch-aws) (AWS) are also mine, and
dormant. This is the current one.
