# Roadmap

Where Maker-5 is heading: what is done, what is next, and what is only
an idea. Each step links to the decision behind it.

## Now

- The three repositories, each from its D-LAB-5 template: the cloud portal,
  the bench edge, and these docs ([ADR-0002](adr/0002-three-repositories.md)).
- A seed T-Box for hardware-software co-design and agent provenance, with its
  SHACL shapes.
- The bench edge's workbench: workspaces on the left, the Model, Proposals,
  History and Files tabs, the chat on the right, in English, German and
  French. One folder per product ([ADR-0003](adr/0003-one-folder-per-product.md)),
  every saved state a revision ([ADR-0004](adr/0004-every-saved-state-is-a-revision.md)),
  files given to the chat ([ADR-0005](adr/0005-files-given-to-the-chat.md)),
  proposals decided by a person ([ADR-0006](adr/0006-a-person-decides-a-proposal.md)).

- People by invitation only ([ADR-0007](adr/0007-accounts-by-invitation.md)).
- Material management: an organisation's stock in tables, shared by its workspaces
  ([ADR-0008](adr/0008-stock-lives-in-tables.md)), our codes, existing codes
  and NFC tags ([ADR-0009](adr/0009-codes-and-tags.md)), workspaces taking
  parts, from the site and the edge ([ADR-0010](adr/0010-workspaces-take-from-the-stock.md)).
- Boxes: cartons, bags and drawers with a large label, holding parts,
  assemblies and other boxes, checked in and out on a phone, with their
  firmware, CAD and other files ([ADR-0011](adr/0011-boxes-hold-parts-and-their-artifacts.md)).
- Parts' own codes: an EAN names the article, a GS1 code's serial or a
  printed serial finds the unit ([ADR-0012](adr/0012-a-products-barcode-names-the-article.md)).
- The edge works with prod or stage, each its own folder, and takes its
  workspaces from the cloud ([ADR-0013](adr/0013-each-environment-its-own-folder.md)).
- A Markdown BOM comes into a workspace's model; missing articles are proposed
  for an admin; the edge's hardware canvas wires pins
  ([ADR-0014](adr/0014-bom-import-and-the-hardware-canvas.md)). Next: a `.fzz`
  export, and pin layouts kept with the article in the cloud.
- Labels carry an M5 mark; a print queue packs labels of every size onto plain
  A4 sticker sheets ([ADR-0015](adr/0015-labels-carry-an-m5-mark-and-pack-onto-plain-sheets.md)).
- The hardware canvas takes parts from the Fritzing parts library, fetched on
  the edge and credited ([ADR-0016](adr/0016-parts-from-the-fritzing-library.md)); full screen; the chat places
  parts and draws wires.
- The landing page splits an organisation's workspaces into active (an edge
  used them in 14 days) and passive ([ADR-0018](adr/0018-active-means-an-edge-used-it-in-14-days.md)).

## Next

- **Code:** a workspace links its git repositories; the edge clones them
  into the product's folder, and the cloud keeps the links and released tags.
- **Maker IDEs on the edge:** an Arduino sketch editor with compile and
  upload, a breadboard view in the manner of Fritzing, and others, beside the
  model and using the parts in stock.
- **Product release:** a versioned bundle of a frozen model baseline, the
  software release, the BOM, documents and CAD, and the boxes holding built
  units.
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
