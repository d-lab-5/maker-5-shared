# ADR-0011: Boxes hold parts and their artifacts

Status: **Accepted**

## Context

Parts don't lie loose on a bench. A prototype that is set aside, such as a
doorbell built on a Raspberry Pi 1 while work moves to a Pi 4, goes into a
carton with its SD card still in it. Spare boards go into bags, cables into
drawers. Someone at the shelf must see what a box holds and take something
out without a computer. The firmware, CAD and datasheets for what is in the
box belong with it.

The stock ([ADR-0008](0008-stock-lives-in-tables.md)) records how many there
are and who has them, but not *where* they are.

## Decision

- **A box** is an organisation's carton, bag, folder, drawer or shelf. It may
  be marked for one workspace. It has a name, a place (or the box it sits
  in), a note, links and files.
- **A box holds:**
  - single **units**: a unit is in a box, with a person, installed in another
    unit, or loose at its place;
  - **counted parts**: a quantity of a bulk article;
  - **other boxes**: at most eight deep, and never inside itself.
- **Boxes never hold more than is on hand.** Each article keeps a `boxed`
  count, and checking parts in keeps `boxed ≤ onHand` in the same
  transaction. A box says *where* parts are; it doesn't change how many
  there are, how many are reserved, or how many are free. Counting or using
  up a bulk article can't go below what its boxes hold.
- **Check in** puts a unit, or a quantity, into a box. **Take out** gives it to
  the person who took it. If they name a workspace, it is also reserved for
  that workspace, in the same step
  ([ADR-0010](0010-workspaces-take-from-the-stock.md)). Each step is a
  movement.
- **Labels.** A box gets a label of 63.5 × 38.1 mm, the common address label
  (21 to an A4 sheet), with a 30 mm QR read from a shelf 30 to 40 cm away. Parts keep
  their small labels or their own barcode
  ([ADR-0009](0009-codes-and-tags.md)).
- **The label host.** A label carries the host of the site that printed it, in
  capitals: `HTTPS://MAKER5.CLOUD/T/K7Q2M9`, or the stage site's host for a
  label printed there. The code reader accepts every host of ours, including
  the earlier `MAKER-5.CLOUD` form, so labels already printed still work.
  This amends the host given in ADR-0009.
- **Artifacts.** A box keeps typed links (firmware, CAD, schematic, datasheet,
  docs, repository, other; https only) and files of up to 50 MB. The files
  are kept in the organisation's storage under the box. They are uploaded and
  downloaded through short-lived signed URLs, so a file never passes through
  the API.
- **Who does what.** Everyone in the organisation works with boxes. Deleting
  one is an admin's step, and only an empty box can be deleted.
- **The edge** reads boxes and their artifacts, so the bench chat can answer
  "which box holds the Pi 1 prototype, and where is its firmware?". It does
  not check parts in or out. That happens on a phone, at the box.
- (The intake lets an edge check items in for the person at its page, an admin: [ADR-0022](0022-intake-scanned-at-the-shelf.md).)
- (Amended: a box may also be a **cabinet** or a **workbench**, the top of a
  workshop that holds shelves and boxes. An admin makes **boxes in a batch
  with their labels**: numbered, as "Box 01" to "Box 21" for a sheet, or
  named one a line, each bound to a fresh code at once. The number stays on
  the box while its contents change; the contents and which box sits where
  come later, by scanning in the intake.)

## Consequences

- Scanning a box's label opens its card on a phone: what it holds, check in
  by scanning each part, take out, move the box, and its links and files.
- A unit's card shows the box it is in, or the person who has it.
- A workspace's overview lists its boxes and their links and files.
- A later **product release** (a "big box" of hardware and software release,
  BOM and documents) refers to boxes, links and files by their ids. Boxes
  carry no release field of their own.
