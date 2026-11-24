# Maker-5

An open-source cloud-and-edge lifecycle workbench for designing, building, and
operating hardware and software products, guided by a shared semantic graph
and human-in-the-loop AI agents.

Maker-5 is a product engineering platform built on the D-LAB-5 cloud-and-edge
architecture. It brings physical hardware prototyping and software development
into one continuous lifecycle loop: **Observe → Design → Build → Run**.

- **Cloud Lifecycle Portal** ([maker-5-cloud](https://github.com/d-lab-5/maker-5-cloud), at maker5.cloud):
  manages your product portfolio, system blueprints, architecture decisions
  and AI agent proposals in one semantic graph. Agents analyse requirements and
  propose changes. A person reviews and approves every step.
- **Local Bench Edge** ([maker-5-edge](https://github.com/d-lab-5/maker-5-edge)):
  runs on your workstation or lab bench. It pairs securely with the cloud and
  watches local Git repositories, CAD/PCB schematics, firmware builds and
  connected test hardware. Knowledge syncs both ways, and your local
  environment is never exposed.
- **HW + SW ontology shard** ([the ontology](ontology/)): links hardware blocks,
  interfaces, software modules and provenance records with open standards
  (OSLC SysML v2, W3C PROV-O and SPDX). Automated SHACL gates enforce it.

This site holds Maker-5's decisions, its roadmap, its ontology, and drawings of
how it is built and how it works. It is public, and holds no one's data.

- **[Diagrams](diagrams/)**: {diagrams} interactive drawings.
- **[Decisions](adr/)**: {adrs} architecture decision records.
- **[Roadmap](roadmap.html)**: where Maker-5 is heading.
- **[Ontology](ontology/)**: the T-Box, its rules and its vocabularies.
