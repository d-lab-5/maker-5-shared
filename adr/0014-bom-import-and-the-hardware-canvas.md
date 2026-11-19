# ADR-0014: A BOM comes into the model, and hardware is wired on the edge's canvas

Status: **Accepted**

## Context

The first real product, a door bell, began as a bill of materials written in
Markdown. It lists the parts already there, in hand and still to buy, and the
software on each device. Typing each line into the model by hand repeats
work. Many of the parts aren't in the organisation's stock yet. And the next
step after a parts list is the wiring: which pin goes to which.

An edge reads the stock but can't write the catalogue, because a device token
is never an admin.

## Decision

- **A BOM is imported on the edge** (`bom_import.py` in maker-5-edge):
  - **Input.** `## Hardware` and `## Software` sections, `###` groups, and a
    table in each.
  - **Hardware rows** become blocks:
    - their group sets a supply, `m5:supplyState`: there, in hand or to buy;
    - their notes go into `schema:description`.
  - **Tool rows** become no block, only an article.
  - **Software rows** become modules:
    - each has its version, `schema:softwareVersion`;
    - each records the block it runs on, `m5:runsOn`, matched from its group
      ("On the Pi").
  - **Preview first.** The import shows what it would make, the person
    adjusts it, and it is written as one revision.
  - **Running it again** skips what the model already has, by name.
- **An edge may propose articles.** The new edge route `materials/propose`:
  - makes the articles a BOM needs and the stock lacks;
  - gives them no stock, and marks each proposed with who proposed it and
    when;
  - returns an existing article where the name is already taken;
  - allows at most 50 articles per call and 200 waiting per organisation.
  
  An admin accepts or deletes each one on Materials. Until it is accepted,
  nothing is received, reserved or labelled for it. This is the edge's only
  write to the catalogue.
- **Pins and wires are in the model** (T-Box 0.3.0):
  - **`m5:Pin`:** `m5:pinOf` a block, a number, a name, and an `m5:pinRole`
    (power, ground, signal, audio, video or data);
  - **`m5:Wire`:** exactly two `m5:wireEnd` pins, a colour, and optionally the
    interface it belongs to (`m5:alongInterface`);
  - **canvas position:** `m5:canvasX` and `m5:canvasY` on a block.
  
  So the wiring syncs with the cloud like the rest of the model, and the chat
  reads it (`canvas()`) and proposes wires (`wire()`). Removing a block takes
  its pins and their wires with it.
- **The canvas is our own,** an SVG page on the edge:
  - **Parts:** a placed block gets the pins of its part from a small library
    (`parts/library.json`: data, not code), or two generic pins. Its pins can
    be edited.
  - **Wiring:** click a pin, then another, to draw a wire; drag a part to move
    it.
  - **Not Fritzing.** Fritzing is a desktop application and can't live in the
    edge's page, and its files would sit beside the model rather than in it.
    A `.fzz` export can follow.

## Consequences

- The edge's model can now say what is still to buy. The cloud's Materials
  page shows the proposed articles waiting for an admin.
- A block's pins are created when it is first placed, not when it is made, so
  a model without a canvas stays as small as before.
- The parts library grows by editing JSON. A later version may take pin
  layouts from the article in the cloud, so every edge shares them.
