# ADR-0008: A workspace's stock lives in tables, not in graphs

Status: **Accepted**

## Context

A workspace keeps parts: boards, SD cards, resistors, cables, tools. It needs
one catalogue of everything in scope, an internal "marketplace", with how many
are on hand, how many products reserved, and how many are free. Some parts are
counted in bulk; others are tracked one by one, each with its own code (this
Pi, this SD card) and its own history.

Products' models are RDF graphs (ADR-0007 in maker-5-cloud), and the first
thought is to put the stock there too. But counts change on every reservation,
two people can reserve the last one at the same moment, and a graph is saved
whole, with an optimistic version, up to 350 KB.

## Decision

- The stock is kept in **DynamoDB tables** in the cloud, owned by the
  workspace (tenant): articles, units, what each product took (allocations),
  a movement log, and codes.
- An article keeps `onHand`, `reserved` and `free` (= on hand − reserved).
  Every step is **one transaction**: the counts or the unit, the product's
  allocation, and a log entry. A reservation is a conditional write,
  `free >= qty`, so the last part cannot be taken twice.
- An article is tracked **in bulk** (a count, in pieces, metres, grams …) or
  **unit by unit** (pieces with their own code, serial number, place, the unit
  they sit in, and the firmware last written to them).
- One function, `materials`, is the only writer for people; `edgeMaterials`
  for edges. Reads go through them too, because who may read depends on
  membership in any of the workspace's products, which a static rule cannot
  express.
- Articles and units still have IRIs, `https://maker-5.cloud/id/article/a-…`
  and `…/unit/u-…`, and the ontology knows `m5:Article`, `m5:Unit`,
  `m5:usesArticle`, `m5:quantity`, `m5:installedIn` and
  `m5:firmwareVersion`: a product's model says which article a block is built
  from.

## Consequences

- Two kinds of store, each for what it is good at: the model as a graph, the
  stock as counters.
- The catalogue is per workspace. A shared, cross-workspace marketplace is a
  later decision.
- Whoever is in the workspace reads its stock; its admins edit the catalogue,
  receive and count stock, and retire units, with two-step sign-in.
