# ADR-0010: Workspaces take parts from the organisation's stock

Status: **Accepted**

## Context

An organisation's workspaces, each a project or a product in development,
work side by side and draw on the same parts.
Who has which Pi, how many SD cards a prototype used, and what is still free
must be clear, from the site, the bench edge and its chat.

## Decision

- A workspace **reserves** parts: a quantity of a bulk article, or a chosen
  unit. It then **uses** them (built in, used up), **releases** a
  reservation, or **returns** what it used.
- Each workspace keeps an allocation per article: reserved and used. A unit
  taken by a workspace names it until it is released or returned.
- Every step is a **movement** in an append-only log: what, how many, which
  workspace, who (an e-mail, or `edge:<id>`), when, and a note. Installing a
  unit in another (the SD card in the Pi), noting the firmware written to it,
  and moving it are movements too.
- Readers of a workspace take parts for it; the organisation's admins for any.
- The **edge** reads the stock and reserves, releases and returns parts for a
  workspace: a product folder on the edge is the cloud workspace of the same
  name. It never edits the catalogue, receives stock or mints codes. In the model, a block or module names the
  article it is built from (`m5:usesArticle`, `m5:quantity`), so the edge
  lists what a workspace needs against what it took and what is free. Its chat
  proposes these steps, each confirmed by the person.

## Consequences

- The stock page shows, per article, which workspaces hold it; the workspace's
  Materials view shows what it took.
- Using up parts is done on the site, not from the edge: it removes them from
  the stock for good.
