# ADR-0016: Parts from the Fritzing library, fetched on the edge

Status: **Accepted**

## Context

The hardware canvas ([ADR-0014](0014-bom-import-and-the-hardware-canvas.md)) gives a
block pins from a small library of our own. The Fritzing parts library
(github.com/fritzing/fritzing-parts) has about 2,000 parts, each with its
connectors and a breadboard picture, and it is open. But it is licensed
**CC BY-SA 3.0**, which is not compatible with the GPL-3 of maker-5-edge:
only CC BY-SA 4.0 may be carried into GPL-3 code.

## Decision

- **Nothing of the library is in our repositories.**
  - The edge searches the library's file list (core and contrib, not
    obsolete). It reads the list with one request to GitHub and keeps it a
    week.
  - It fetches only the part the person picks, and that part's breadboard
    picture, from raw.githubusercontent.com into its own cache,
    `<data>/fritzing/`.
  - Only files named in that list are fetched.
- **A block takes a part** (`m5:fritzingPart`, the part's file name, ontology
  0.4.0). The part's connectors become the block's pins. The model keeps
  only the file name and those pins, never the part's files.
- **Pin numbers:** Fritzing keeps none, and a board's connectors come in any
  order. Where our own library knows the board (the Raspberry Pi 1 B header)
  and every pin name matches, the board's real numbers are used. Otherwise
  the part's connector order is kept, which is right for ICs.
- **The picture** is drawn inside the part on the canvas, as an SVG `<image>`,
  which runs nothing. It is also served with a policy that allows nothing.
- **Credit:** wherever a part is used, its title, authors, licence and source
  are shown, and the canvas says that its parts come from the Fritzing parts
  library under CC BY-SA 3.0.
- The chat uses it too: `fritzing_search()`, `fritzing_part()` and
  `fritzing_use()`, confirmed like any change.

## Consequences

- The first use of a part needs the network. After that it comes from the
  cache.
- Pins are placed in rows beside the picture, not on it. Putting them at
  their spots on the drawing would mean reading each SVG's geometry, which
  is left for later, as is a `.fzz` export.
- Another edge sees the block's pins and the part's name at once. It fetches
  the picture the first time it draws it.
