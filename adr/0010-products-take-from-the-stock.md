# ADR-0010: Products take parts from the workspace's stock

Status: **Accepted**

## Context

A workspace's products are developed side by side and draw on the same parts.
Who has which Pi, how many SD cards a prototype used, and what is still free
must be clear, from the site, the bench edge and its chat.

## Decision

- A product **reserves** parts: a quantity of a bulk article, or a chosen
  unit. It then **uses** them (built in, used up), **releases** a
  reservation, or **returns** what it used.
- Each product keeps an allocation per article: reserved and used. A unit
  taken by a product names it until it is released or returned.
- Every step is a **movement** in an append-only log: what, how many, which
  product, who (an e-mail, or `edge:<id>`), when, and a note. Installing a
  unit in another (the SD card in the Pi), noting the firmware written to it,
  and moving it are movements too.
- Readers of a product take parts for it; the workspace's admins for any.
- The **edge** reads the stock and reserves, releases and returns parts for a
  product, matched to its space by name; it never edits the catalogue,
  receives stock or mints codes. In the model, a block or module names the
  article it is built from (`m5:usesArticle`, `m5:quantity`), so the edge
  lists what a product needs against what it took and what is free. Its chat
  proposes these steps, each confirmed by the person.

## Consequences

- The stock page shows, per article, which products hold it; the product's
  Materials view shows what it took.
- Using up parts is done on the site, not from the edge: it removes them from
  the stock for good.
