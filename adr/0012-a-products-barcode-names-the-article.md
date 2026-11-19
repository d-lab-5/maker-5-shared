# ADR-0012: A product's barcode names the article, its serial the unit

Status: **Accepted**

## Context

Most bought parts already carry a code. Our own labels
([ADR-0009](0009-codes-and-tags.md)) should be needed only for things without
one, such as SD cards, cables and our own boards.

These codes don't all say the same thing:
- **An EAN or UPC names a product.** Every box of the same board carries the
  same one. Binding it to one unit made the next box of that board say the
  code was taken.
- **GS1 codes carry more than one field.** The DataMatrix and GS1-128 codes
  on boards, modules and reels, and GS1 Digital Link URLs, pack the product's
  GTIN together with a serial number (AI 21) and a batch (AI 10). Kept as
  plain text, the serial was lost.
- **A serial number is often printed on its own,** as a Code 128 or a MAC
  address sticker, and needs no binding to find its unit.

## Decision

- **A GTIN (EAN, UPC, GS1) is always an alias of an article.** Binding one
  for a unit puts it on the unit's article instead. Binding it to the article
  it already names is no error: that is the next box of the same product.
- **A GS1 code is read into its fields:** the GTIN, the serial number and the
  batch. This works for the scanned form (with GS separators and a symbology
  prefix), the printed `(01)…(21)…` form, and a Digital Link URL.
- **The GTIN finds the article, the serial the unit.** If the serial matches
  exactly one live unit of that article, the code opens that unit. If not, it
  opens the article, which asks **which one?**: its units with where each is,
  or, for an admin, a new unit with the code's serial. Binding a GS1 code for
  a unit stores the serial on that unit, so the same code finds it next time.
- **An unbound code matching exactly one live unit's serial opens that
  unit.** No binding is needed for a serial barcode or a MAC sticker.
- **When checking parts into a box** ([ADR-0011](0011-boxes-hold-parts-and-their-artifacts.md)),
  a product barcode of an article tracked unit by unit asks which unit. A
  code nobody knows is bound on the box card, and scanning carries on.

## Consequences

- One scan of a bought part's own code is usually enough. Our small labels are
  for parts with no code of their own.
- A serial number shared by two units of an organisation finds neither. The
  person picks the unit from the article.
