# Roadmap

Where Maker-5 is heading: what is done, what is next, and what is only
an idea. Each step links to the decision behind it.

## Now

- The three repositories, each from its D-LAB-5 template: the cloud portal,
  the bench edge, and these docs ([ADR-0002](adr/0002-three-repositories.md)).
- A seed T-Box for hardware-software co-design and agent provenance, with its
  SHACL shapes.

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
