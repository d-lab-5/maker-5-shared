# ADR-0013: Each environment its own folder; workspaces come from the cloud

Status: **Accepted**

## Context

Maker-5 runs as two clouds, production at maker5.cloud and stage at
stage.maker5.cloud, and an edge may be linked to both. Until now the edge
kept one folder of products (`mydocs/`) and synced each with every linked
cloud in turn, so a change made against stage reached production on the next
round. Products could also be started on the edge alone, and were matched to
a cloud workspace by name. A product that had no cloud workspace of the same
name never synced, and renaming a workspace in the cloud cut its product off.

## Decision

- **An environment is a connection** (`prod`, `stage`). Each has its own
  folder, `mymaker5docs/prod/` and `mymaker5docs/stage/`, with its products,
  their revisions and files, and the chat's proposals and history. It syncs
  with its own cloud alone; nothing passes between stage and prod on the
  edge. Promoting work from stage to prod is a separate, explicit step, for
  example the product release on the roadmap.
- **The left bar switches environment.** An environment that isn't linked
  yet stays in the list and offers to link it. Its site is enough to link:
  the cloud publishes its edge API address at
  `/.well-known/dlab5-edge.json`.
- **Workspaces are made in the cloud,** by an admin of the organisation. The
  edge lists the current environment's workspaces and gives each its folder.
  It refreshes the list at start, on switching, and on request. Starting a
  workspace on the edge is refused, with a link to the cloud.
- **A workspace is found by name once, then by its id,** kept per connection.
  A rename in the cloud keeps the same folder. A workspace removed in the
  cloud is marked "removed in the cloud"; its folder is never deleted on the
  edge.
- An older `mydocs/` becomes `mymaker5docs/prod/` on the edge's first start.
- The behaviour comes from template-dlab5-edge's `SYNC_SCOPE =
  "per-connection"`. Its default stays `"merged"` for products whose stage and
  prod should meet.
- (Amended at the go-live: **one way across.** A person can fill an *empty*
  workspace from another connection's, as stage's door bell into a new prod
  workspace. It is started on the edge's page, shown as a dry run first, and
  copies once, one way. Only the open model travels, what a repository
  publishes ([ADR-0017](0017-design-and-code-are-open-on-github.md)): blocks,
  modules, interfaces, pins, wires and pictures, with their ids. Stock links
  and quantities, proposals, decisions, provenance and revisions stay with
  their cloud. The chat can't start one.)

## Consequences

- [ADR-0003](0003-one-folder-per-product.md)'s folder is now one per
  environment: `mymaker5docs/<environment>/<product>/`.
- A workspace in stage and one in prod are different workspaces on the edge,
  even with the same name.
- The edge can't create a workspace offline.
