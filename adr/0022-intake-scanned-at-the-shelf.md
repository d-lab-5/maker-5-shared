# ADR-0022: The intake: scanned at the shelf, described at the edge, checked in by a person

Status: **Accepted**

## Context

The workshop is put in order with labels: a shelf holds boxes, a box holds
parts or a quantity of one part. Boxes, blank codes and checking in on a
phone exist already ([ADR-0009](0009-codes-and-tags.md),
[ADR-0011](0011-boxes-hold-parts-and-their-artifacts.md)), but checking a
part in on the phone needs its article first. At the shelf, with a part in
one hand, nobody types a maker, a model and a category.

The house also has two kinds of inventory: parts for making things (Maker-5)
and devices a smart home can use (DigitalHome.Cloud: a PC, a power supply, a
router). A label should say which one it is for.

## Decision

- **A code of ours has a family,** chosen when its batch is minted: `m5` for
  maker parts or `dhc` for smart-home devices. The label prints the family's
  mark, `M5 K7Q2M9` or `DHC K7Q2M9`. The QR is unchanged
  (`HTTPS://<host>/T/K7Q2M9`), and the reader accepts either mark. An
  article may be `family: dhc`; it uses the same categories
  ([ADR-0019](0019-article-categories-in-two-levels.md)). This amends
  [ADR-0015](0015-labels-carry-an-m5-mark-and-pack-onto-plain-sheets.md).
- **Capture first.** On the phone, Scan → Intake:
  1. Scan the place, a shelf or a box. A blank label scanned here becomes a
     new shelf or box.
  2. Scan label after label of what goes into it, each with up to three
     photos, a note and a count.

  Every scan is an **intake item**; nothing is in the stock yet. A label that
  already points at something is captured too, and names that thing. The
  photos are kept in the organisation's storage under `intake/` and pass
  only through signed links.
- **The edge describes.** Its agent reads each item's photos and note with
  the `intake` skill. It searches the stock first and reuses an article that
  fits. It then suggests what the item is:
  - **a device:** one unit of an article tracked unit by unit, with its
    serial if a photo shows one;
  - **a box of parts:** a new box in the place, holding a quantity of a
    counted article;
  - **loose parts:** a quantity counted into the place.

  For a new article it also suggests the name, category, maker and model, and
  says how sure it is of each field. A suggestion changes nothing in the
  stock.
- **A person checks in,** on the edge's Intake tab or the site.
  - Checking in is **one transaction**. The article is found or made, and a
    made article is marked `intake`. The stock is received. Then a unit goes
    into the place, a new box of parts goes into the place, or the parts are
    counted into it. Finally the label is bound and the item is closed.
  - A known label instead moves its unit or box into the place.
  - `boxed ≤ onHand` holds as before.
- **Who.** Anyone in the organisation captures and checks in, as with boxes.
  This includes the edge, for the person at its page. This relaxes two
  earlier rules:
  - Making an article and receiving stock were an admin's steps with two-step
    sign-in. Through the intake they are not, so anyone in the organisation
    can now add stock this way.
  - [ADR-0010](0010-workspaces-take-from-the-stock.md) and
    [ADR-0011](0011-boxes-hold-parts-and-their-artifacts.md) said an edge
    never checks in and never writes codes. For the intake it does both.

  The agent can only describe. Check in and drop are not among the calls the
  chat may propose, and the edge refuses them from an agent.

## Consequences

- An evening at the shelf is scanning and photos; the describing waits for
  the bench, where the agent does most of it.
- Admins see which articles came from the intake, and can tidy their names
  and categories.
- A later export to DigitalHome.Cloud reads the `dhc` articles and their
  units.
- The phone no longer needs an article to put a new part in a box.
