# ADR-0006: A proposal is decided by a person, on the page

Status: **Accepted**

## Context

Agents suggest changes to a product: the edge's chat, or an agent through
MCP. Maker-5 is human-in-the-loop: such a suggestion should be on record,
attributed to whoever made it, and accepted or turned down by a person.

## Decision

- An agent records a suggestion as a `m5:Proposal` (a `prov:Entity`), with
  `prov:wasAttributedTo` the agent and `m5:concerns` the parts it touches.
- Only a person on the page approves or rejects it. The decision is a
  `prov:Activity` that `prov:used` the proposal, `prov:wasAssociatedWith` the
  person, with `prov:endedAtTime` and an optional note.
- There is no MCP tool for it, the chat's allowed routes leave it out, and
  the route refuses a request from an agent (`X-Agent`) or a replayed chat
  proposal with 403 (`maker_5_edge/app.py`).

## Consequences

- The model shows who proposed what, who decided and when, in PROV-O, which
  the cloud can read without knowing the edge.
- An agent cannot approve its own proposal, even when asked to.
