# ADR-0002: Three repositories, each with its own licence

Status: **Accepted**

## Context

Maker-5 has a web portal in the cloud, an app that runs on a workstation or
lab bench, and a shared vocabulary both of them use. Each part has different
readers, and each needs a different licence: the portal is used over a
network, the edge is installed and run locally, and the vocabulary and
documents should be reusable by anyone.

## Decision

Maker-5 is split into three public repositories:

| Repository | What it is | Made from | Licence |
|---|---|---|---|
| [d-lab-5/maker-5-cloud](https://github.com/d-lab-5/maker-5-cloud) | The cloud lifecycle portal and semantic graph engine, at maker-5.cloud | template-dlab5-cloud | GNU AGPLv3 |
| [d-lab-5/maker-5-edge](https://github.com/d-lab-5/maker-5-edge) | The local workstation and lab-bench edge | python-tool-template + template-dlab5-edge | GNU GPLv3 |
| [d-lab-5/maker-5-shared](https://github.com/d-lab-5/maker-5-shared) | These docs: decisions, roadmap, diagrams, the ontology reference | template-dlab5-shared | CC BY 4.0 (documents), MIT (scripts) |

The ontology's source, the T-Box and its SHACL shapes, lives in the cloud
under `packages/ontology`, because the cloud validates every save against it.
This repository publishes a copy, made by `scripts/sync-ontology.sh`.

## Consequences

- **AGPLv3 for the cloud** means that anyone who runs a changed portal as a
  service must offer its source to its users.
- **GPLv3 for the edge** keeps changed copies of it open when they are passed on.
- Decisions that concern both cloud and edge are recorded here, so both can
  cite them as "ADR-NNNN in maker-5-shared".
