# ADR-0021: A block's picture: from a library, drawn from its pins, or by the agent

Status: **Accepted**

## Context

A block on the hardware canvas can take a part of the Fritzing parts library,
with its pins and its breadboard picture
([ADR-0016](0016-parts-from-the-fritzing-library.md)). Many parts are not in
that library, and a Fritzing part models only a board's header: a Raspberry
Pi's USB, video, audio, Ethernet and power sockets are drawn but not
connectors, so the cables to a hub or a grabber had nothing to be wired to.

## Decision

- **One Image window per block, one source at a time:**
  - **Fritzing**, as before (`m5:fritzingPart`).
  - **Wokwi**: the Wokwi elements (github.com/wokwi/wokwi-elements, MIT), common
    boards and parts with their pins (`m5:partImage "wokwi:<element>"`). They
    are web components; a script renders each one once into plain SVG with
    its pins, and that snapshot ships with the edge. Nothing is fetched and no
    third-party JavaScript runs on the page.
  - **From its pins**: a plain drawing of the block's own pins, always right
    (`m5:partImage "drawn"`).
  - **By the agent**: an SVG the agent draws from the datasheet or the
    person's files (`m5:partDrawing`, at most 40 000 characters).
  - **None**: no picture; the pins stay.
- **A library part brings its pins, and a board keeps its ports.** The edge's
  pin library lists a board's ports as pins with the roles data, video, audio
  and power; taking a library part keeps them after its header. A cable is a
  wire between ports. The library can also name the Fritzing part that fits a
  board, which the window offers first.
- **The agent draws SVG, not images.** Its drawing is shapes and text only,
  with a viewBox and no references, scripts, styles or entities, and each of
  the block's pins is marked exactly once with `data-pin`. The edge checks it
  before it is proposed and again before it is kept, and says what is wrong.
  The agent learns the rules from the `part-image` skill, which the edge's
  MCP server returns (`image_skill()`), and looks in the libraries first.
- **Every picture is shown as an `<image>`** and served with a Content
  Security Policy that allows nothing to run.
- **Every picture is part of the design.** A push writes
  `hardware/images/<block>.svg` and credits the sources in `NOTICE`: Fritzing
  (CC BY-SA 3.0), Wokwi (MIT), and the drawn ones with the hardware licence.

## Consequences

- A picture cannot claim a pin the block does not have, or hide one it has.
- An image-generation model was left out: its output cannot be checked
  against the pins, and the licence of generated pictures is unsettled, while
  these pictures are published.
- The Wokwi snapshot is updated by re-running the script when Wokwi releases;
  the elements are not followed automatically.
- The agent's drawing is in the model, so it syncs and is in the history,
  like any other change.
