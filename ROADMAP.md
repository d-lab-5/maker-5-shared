# Roadmap

Where Maker-5 is heading: what is done, what is next, and what is only
an idea. Each step links to the decision behind it.

## Now

- The three repositories, each from its D-LAB-5 template: the cloud portal,
  the bench edge, and these docs ([ADR-0002](adr/0002-three-repositories.md)).
- A seed T-Box for hardware-software co-design and agent provenance, with its
  SHACL shapes.
- The bench edge's workbench: products on the left, the Model, Proposals,
  History and Files tabs, the chat on the right, in English, German and
  French. One folder per product ([ADR-0003](adr/0003-one-folder-per-product.md)),
  every saved state a revision ([ADR-0004](adr/0004-every-saved-state-is-a-revision.md)),
  files given to the chat ([ADR-0005](adr/0005-files-given-to-the-chat.md)),
  proposals decided by a person ([ADR-0006](adr/0006-a-person-decides-a-proposal.md)).

- People by invitation only ([ADR-0007](adr/0007-accounts-by-invitation.md)).
- Material management: a workspace's stock in tables
  ([ADR-0008](adr/0008-stock-lives-in-tables.md)), our codes, existing codes
  and NFC tags ([ADR-0009](adr/0009-codes-and-tags.md)), products taking
  parts, from the site and the edge ([ADR-0010](adr/0010-products-take-from-the-stock.md)).

## Next

- **Observe:** the edge watches local Git repositories and firmware builds, and
  reports them to the cloud's graph.
- **Design:** products, system blueprints and architecture decisions in the
  portal, linked to SysML v2 blocks and interfaces.
- **Agent proposals:** an agent proposes a change to the graph; a person
  approves or rejects it, and the decision is recorded with PROV-O.

## Later

- **Build:** CAD/PCB artifacts and SPDX bills of materials tied to the blocks
  they realise.
- **Run:** telemetry from connected test hardware on the bench, fed back into
  the next Observe step.
