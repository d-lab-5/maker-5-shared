# ADR-0003: One folder per product on the edge

Status: **Accepted**

## Context

The edge keeps a person's products on their own computer. A product has more
than its model: the files given to the chat, and every saved state of the
model. When all products shared one folder of graphs, those had nowhere
obvious to go, and removing or copying a product meant picking files out of
several places.

## Decision

(Since [ADR-0013](0013-each-environment-its-own-folder.md) the data folder is
`mymaker5docs/<environment>/`, one per cloud.)

Each product is one folder in the edge's data folder (`mydocs/` by default):

```
mydocs/<product>/model.ttl        the product's model graph (the A-Box)
mydocs/<product>/files/<id>/      files given to the chat (ADR-0005)
mydocs/<product>/revisions/       every saved state (ADR-0004)
```

- A product's id (`p-` and eight characters) is minted once and never
  changes. Its title can change; the folder stays.
- `abox`, `inbox`, `files` and `revisions` are reserved and never a product.
- The edge layer's cloud sync reads the same layout: `app.main()` installs
  `ProductKB` as the knowledge base (`maker_5_edge/products.py`).
- An edge with the older single-folder layout (`mydocs/abox/`) is moved once,
  on start, by `migrate_layout()`.

The pattern comes from digitalhome-pro-edge, which made the same move for
its workspaces.

## Consequences

- A product can be backed up, copied or removed as one folder.
- Folders are private to the person (0700).
