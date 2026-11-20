# ADR-0008: An organisation's stock lives in tables, not in graphs

Status: **Accepted**

## Context

Maker-5 calls a tenant an **organisation** and each of its spaces a
**workspace**: a project or a product in development.

An organisation keeps parts: boards, SD cards, resistors, cables, tools. It needs
one catalogue of everything in scope, an internal "marketplace", with how many
are on hand, how many workspaces reserved, and how many are free. Some parts are
counted in bulk; others are tracked one by one, each with its own code (this
Pi, this SD card) and its own history.

Workspaces' models are RDF graphs (ADR-0007 in maker-5-cloud), and the first
thought is to put the stock there too. But counts change on every reservation,
two people can reserve the last one at the same moment, and a graph is saved
whole, with an optimistic version, up to 350 KB.

## Decision

- The stock is kept in **DynamoDB tables** in the cloud, owned by the
  organisation (the tenant): articles, units, what each workspace took (allocations),
  a movement log, and codes.
- An article keeps `onHand`, `reserved` and `free` (= on hand − reserved).
  Every step is **one transaction**: the counts or the unit, the workspace's
  allocation, and a log entry. A reservation is a conditional write,
  `free >= qty`, so the last part cannot be taken twice.
- An article is tracked **in bulk** (a count, in pieces, metres, grams …) or
  **unit by unit** (pieces with their own code, serial number, place, the unit
  they sit in, and the firmware last written to them).
- One function, `materials`, is the only writer for people; `edgeMaterials`
  for edges. Reads go through them too, because who may read depends on
  membership in any of the organisation's workspaces, which a static rule cannot
  express.
- Articles and units still have IRIs, `https://maker-5.cloud/id/article/a-…`
  and `…/unit/u-…`, and the ontology knows `m5:Article`, `m5:Unit`,
  `m5:usesArticle`, `m5:quantity`, `m5:installedIn` and
  `m5:firmwareVersion`: a workspace's model says which article a block is built
  from.

## Consequences

- Two kinds of store, each for what it is good at: the model as a graph, the
  stock as counters.
- The catalogue is per organisation. A shared, cross-organisation marketplace is a
  later decision.
- Whoever is in the organisation reads its stock; its admins edit the catalogue,
  receive and count stock, and retire units, with two-step sign-in.
