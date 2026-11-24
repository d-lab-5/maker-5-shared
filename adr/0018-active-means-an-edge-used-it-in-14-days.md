# ADR-0018: Active means an edge used it in 14 days

Status: **Accepted**

## Context

The edge is where a workspace is worked on: the agent and the person design
its hardware and write its software there. The cloud is the second brain
behind it. The cloud's landing page should show which workspaces are being
worked on and which are resting, because a resting workspace's parts can be
reused in another design.

## Decision

- **A workspace is active** while an edge has read or written its graph in
  the last **14 days**. Every other workspace is passive.
- **The cloud notes the touch.**
  - When an edge reads a graph (`graphs/get`), the cloud writes
    `edgeSeenAt`, `edgeSeenBy` (`edge:<id>`) and `edgeSeenName` (the edge's
    host name) on the space's Graph row. It writes at most once an hour
    (a conditional update), and only on a graph that exists.
  - A save from an edge (`graphs/put`) is a touch too.
  - A save from the browser keeps the touch: the store writes with an
    update, not a put.
- **Each save counts the blocks and software modules** in the graph, so the
  landing page can show them without reading the Turtle.
- **The landing page** shows one organisation at a time, chosen by the
  person and remembered in the browser. It is split into an **Active** half
  (latest edge use first) and a **Passive** half (latest change first).
  Each card says which edge used it and when, the last change, and the
  counts. On a phone the halves stack.
- The edge needs no change: it already syncs every workspace at start, on
  switching and on Refresh.

## Consequences

- An edge that is switched off makes its workspaces passive after two weeks,
  without anyone marking them.
- A workspace that only the browser edits stays passive, by design.
- Reading the activity is a filtered list of the tenant's Graph rows. That
  is fine for tens of workspaces; an index on `tenantId` can come later.
