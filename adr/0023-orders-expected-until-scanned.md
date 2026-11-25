# ADR-0023: Orders: read from the order e-mail, expected until the parcel is scanned

Status: **Accepted**

## Context

Most parts are bought online, many of them on Amazon. Typing each one in
again, with its title and its order number, is the step people skip. Then
the stock doesn't know what is on its way, and the intake
([ADR-0022](0022-intake-scanned-at-the-shelf.md)) starts every parcel from a
photo.

An ordinary Amazon account has no order API; only Amazon Business has one.
Signing in to a shop on the person's behalf is ruled out: it needs their
password, and the shops forbid it. What every order does leave is a
confirmation e-mail.

## Decision

- **The source is the order-confirmation e-mail.** The person drops it
  (`.eml`, saved from the mail program) on the edge's Intake tab, or pastes
  its text. The edge reads it with the standard library, Amazon's format
  first.
- **Only the order leaves the e-mail:** the shop (from the sender's domain),
  the order number and its date, and for each line its title, the shop's
  article number (an ASIN), the quantity and the unit price. The address,
  the name, the payment and the shop's suggestions after the order are read
  past. The e-mail is not kept.
- **A preview first, writing nothing.** Each line is matched to an article
  of the stock, by the shop's article number and then by name, or a new
  article is drafted, with its category from the title's words. A pack in
  the title ("100 Stück") is counted in pieces. The person ticks the lines
  to keep, and can change the counts.
- **A kept line is expected** in the organisation's stock, in the cloud
  (`MkOrderLines`). The same line imported twice is one line: the key is the
  shop, the order number and the article number. Anyone in the organisation
  imports and drops lines. A line is never written to a workspace's public
  repository ([ADR-0017](0017-design-and-code-are-open-on-github.md)), as
  prices aren't.
- **The stock moves only at check-in.** When the parcel comes it is scanned
  at the shelf as before. The agent, or the person, names the item's order
  line (`orderLineId`). The line brings the article, what it is and the
  quantity, and the describer gives only what differs. The admin's check-in
  books the stock and counts the line as arrived in the same transaction.
  A line arrives in one item or in several.
- **An article remembers where it was bought:** `supplierRefs`, such as
  `amazon.de:B0…`, at most five, set at check-in from the line. The next
  order of the same part then matches it by the shop's number. This amends
  [ADR-0010](0010-workspaces-take-from-the-stock.md).

**Amended by [ADR-0024](0024-supplier-documents-onto-the-parts.md):** every
supplier document comes in through one door (an e-mail, a PDF, the chat's
📎): lines are expected, linked to a part the stock has, or skipped, and the
attached documents are kept on the parts. The chat may propose the import.

## Diagrams

- [From the order to the shelf](../diagrams/orders.html): a supplier's
  document on the edge, expected or linked to a part we have, and the
  check-in that books it and moves its documents onto the part.

## Consequences

- What is on its way shows under Expected on the Intake tab, on the edge and
  on the phone.
- A parcel's items mostly describe themselves: the line knows the article.
- Other shops (Reichelt, Mouser, AliExpress) need a parser each. The order
  line is the same.
- Not now: a forwarding address that reads the e-mails by itself (the line
  model is ready for it), and the Amazon Business API.
