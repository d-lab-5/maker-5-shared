# CLAUDE.md: Maker-5 shared documentation

The public home of Maker-5's decisions, roadmap, diagrams and ontology reference,
built into docs.dlab5.net/d-lab-5/maker-5/ and published at docs.maker-5.cloud.
It describes: d-lab-5/maker-5-cloud, d-lab-5/maker-5-edge.

> **This repository is public.** Read SECURITY.md. No A-Box data, no real
> names, no account ids, hostnames of cloud resources, keys or local paths.
> `npm run check` sweeps for them; it cannot catch a real name in prose.

## Commands

```bash
npm ci
npm run diagrams            # render what changed; needs the Archify skill
npm run check               # what CI runs: shared.json, diagrams, site, public sweep
npm run serve               # build and serve _site/ on 127.0.0.1:8792
./start-claude.sh           # Claude Code here, with the app repositories beside this one added
```

## Things that bite

1. **The scripts, site/site.css, the icon, the workflow, SECURITY.md and LICENSE belong
   to template-dlab5-shared.** `copier update --trust` replaces them; change
   them there. shared.json and the content are this repository's.
2. **A diagram is listed exactly once**, in `diagrams/README.md` under a group or
   in its ADR's `## Diagrams`, as `- [Title](../diagrams/<name>.html): what it
   explains.` That line is the gallery card's text. `check` fails otherwise.
3. **Never edit a rendered `diagrams/*.html`.** Edit the source and run
   `npm run diagrams`; `check` notices a page changed by hand.
4. **Facts in a diagram come from the code**, read first in the app
   repositories. Cite paths in the cards.
5. **A private app repository is named, never linked** (a trailing `*` in the
   Copier answers, `"private": true` in shared.json). `check` refuses the link.
6. **The ontology's source is in ../maker-5-cloud/packages/ontology.** Change it there, then
   run `scripts/sync-ontology.sh` here.

## Conventions

Write for a reader who was not there: short sentences, plain words. `stage`
integrates, `main` is promoted by PR; docs.dlab5.net rebuilds every night.
