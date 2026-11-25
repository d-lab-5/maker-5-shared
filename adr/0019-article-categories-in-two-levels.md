# ADR-0019: Article categories in two levels, as SKOS

Status: **Accepted**

## Context

The stock is the organisation's inventory, shared by its workspaces. People
and the edge's agent should browse it the way they browse a shop: broad
categories first, then narrower ones, then the parts. The first eight
categories (compute, storage, electronic, mechanical, cable, consumable,
tool, other) were one level and too coarse. A sound card, for example, had
nowhere to go but storage.

## Decision

- **Two levels**, kept as data in the ontology: `m5:ArticleCategories` has
  13 top concepts with `skos:narrower` subcategories, each labelled in en,
  de and fr. Ontology 0.5.0.

| Top (`skos:notation`) | Subcategories |
|---|---|
| Controllers & computers (`controllers`) | sbc, mcu, radio-modules, storage, phones |
| Sensors (`sensors`) | environment, motion, vision, buttons, electrical |
| Actuators (`actuators`) | motors, servos, relays, locks, pumps |
| Power (`power`) | supplies, converters, batteries, protection |
| Communication (`communication`) | networking, radio, buses |
| Displays & media (`media`) | displays, audio, video, lights |
| Electronic components (`components`) | ics, semiconductors, passives, prototyping |
| Cables & connectors (`cables`) | cables, connectors, wire |
| Chassis & enclosures (`chassis`) | enclosures, frames, wheels, printed |
| Mechanical (`mechanical`) | fasteners, bearings, mounting |
| Tools (`tools`) | hand, measuring, soldering |
| Consumables (`consumables`) | solder, tape, filament, heat-shrink |
| Other (`other`) | |

- **An article carries a notation:** `top` or `top/sub`, e.g.
  `controllers/sbc`. A filter on a top category holds its subcategories.
- **One source:** the cloud's `backend/scripts/sync-ontology.mjs` reads the
  concepts and writes the list for the Lambdas and for the site
  (`packages/core`), so the two never differ. The edge reads its own copy of
  the ontology.
- **The old values are mapped when read** and on the next save: compute →
  `controllers/sbc`, storage → `controllers/storage`, electronic →
  `components`, mechanical → `mechanical`, cable → `cables`, consumable →
  `consumables`, tool → `tools`. No migration job is needed.
- **The site's Materials page opens on tiles** of the top categories, with
  article and free counts. A tile leads to its subcategories, and a
  breadcrumb leads back. The list below follows the chosen category, and a
  search counts across all of them.
- **The edge's agent** sees the same: `catalogue()` gives the tiles, and
  `free_parts(category)` gives free articles plus the parts that passive
  workspaces hold ([ADR-0018](0018-active-means-an-edge-used-it-in-14-days.md)).
  The agent is told to reuse before it proposes a new article.
- **A BOM import guesses** the two-level category from whole words of the
  part's name, the most particular first.

## Consequences

- A new subcategory is a change to the ontology, a regenerated list, and a
  deploy. That is deliberate: categories are part of the shared vocabulary.
- Tiles with no articles are shown dimmed, so the shape of the catalogue
  stays the same from one organisation to the next.

## Amendment, 2026-11-25: robots

A fourteenth top category, **Robots** (`robots`), after Controllers, with
`robots/educational` (educational and toy robots), `robots/mobile` (mobile
robots and rovers) and `robots/arms`. A robot has a controller inside, but it
is the robot that is lent, charged and labelled, so it is not filed under
`controllers`. Robots are tracked unit by unit: the edge's guesses and an
order e-mail's draft make them devices.
