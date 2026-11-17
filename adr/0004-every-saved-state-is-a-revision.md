# ADR-0004: Every saved state of a model is a revision

Status: **Accepted**

## Context

A product's model is changed from three places: the page, the chat's
confirmed proposals, and agents through MCP. A person needs to see what
changed, who changed it and through what, and to go back. Hardware also has
states worth naming: a prototype, a release.

## Decision

Every save that changes a model writes a revision in
`mydocs/<product>/revisions/` (`maker_5_edge/revisions.py`):

- `rNNNN.ttl`, the whole graph at that state, and a line in `log.jsonl` with
  when, by whom, through what (`page`, `chat` or `mcp`) and a summary.
- A save that changes nothing writes no revision.
- A revision can be **named** (a tag that can move) or made a **baseline** (a
  frozen name, e.g. "Prototype A", that never moves and cannot be removed).
- Two revisions are compared node by node: what was added, removed, and
  which properties changed.
- **Going back** writes the old state as a new revision. Nothing is lost.

## Consequences

- The History tab and the `revisions`, `diff`, `tag_revision` and `restore`
  MCP tools read the same files.
- Revisions take disk space in proportion to the number of saves; a model is
  small, so this is accepted.
