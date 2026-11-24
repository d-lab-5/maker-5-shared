# Maker-5: shared documentation

The decisions, roadmap, diagrams and ontology reference of Maker-5.
This repository is public and holds only what can be: structures and decisions,
never anyone's data (see [SECURITY.md](SECURITY.md)).

> An open-source cloud-and-edge lifecycle workbench for designing, building,
> and operating hardware and software products, guided by a shared semantic
> graph and human-in-the-loop AI agents.

| Repository | What it is | Licence |
|---|---|---|
| [maker-5-cloud](https://github.com/d-lab-5/maker-5-cloud) | Cloud lifecycle portal and semantic graph engine for Maker-5. It orchestrates hardware and software product design with human-in-the-loop AI agents. | GNU AGPLv3 |
| [maker-5-edge](https://github.com/d-lab-5/maker-5-edge) | Local workstation and lab-bench edge daemon for Maker-5. It syncs Git repos, CAD/PCB artifacts and hardware telemetry with the cloud. | GNU GPLv3 |
| maker-5-shared (this one) | Decisions, roadmap, diagrams, and the RDF/OWL ontology with SHACL shapes for hardware-software co-design and agent provenance | CC BY 4.0 / MIT |

Read it at **https://docs.dlab5.net/d-lab-5/maker-5/**.

## Working on it

```bash
npm ci
npm run diagrams          # render the diagrams whose source changed (needs Archify)
npm run check             # shared.json, diagrams in step, site builds, nothing private
npm run serve             # build _site/ and serve it on http://127.0.0.1:8792/
scripts/sync-ontology.sh  # copy the ontology from ../maker-5-cloud/packages/ontology
```

**Adding a diagram:** write `diagrams/src/<name>.<type>.json`, list it once,
with what it explains, under a group in `diagrams/README.md` or under its ADR's
`## Diagrams`, then `npm run diagrams` and `npm run check`.

**Adding a decision:** `adr/NNNN-short-title.md`, with Context, Decision and
Consequences filled in. Numbers are never reused.

Made from [template-dlab5-shared](https://github.com/d-lab-5/template-dlab5-shared);
`copier update --trust` brings in its fixes.

## Licence

Documents and diagrams CC BY 4.0; scripts and the ontology copy MIT. See
[LICENSE](LICENSE) and [NOTICE](NOTICE).
