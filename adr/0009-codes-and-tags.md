# ADR-0009: Codes and tags for the physical parts

Status: **Accepted**

## Context

A part on the bench has to be found in the stock in a second, by whoever holds
it, with a phone. Many parts already carry a code: an EAN on the box, a
manufacturer's QR. Others have room for almost nothing: an SD card or a SIM
card holding a board's OS, which should still say which card it is and what
was written to it.

## Decision

- **Our codes** are six characters of Crockford base32 (no I, L, O, U),
  minted in batches by an organisation's admins and printed before they belong to
  anything. Someone sticks one on a part and scans it on the bench to bind it
  to an article or a unit.
- (The host is amended by [ADR-0011](0011-boxes-hold-parts-and-their-artifacts.md):
  a label carries the host of the site that printed it.)
- A label's QR holds `HTTPS://MAKER-5.CLOUD/T/K7Q2M9`, in capitals, so the QR
  uses its alphanumeric mode: version 2 at error correction L, 25 modules,
  about **10 mm** on a 600 dpi laser sticker. An **NFC tag** holds the same
  URL; an iPhone opens it without an app, and Chrome on Android writes and
  locks it (Web NFC), after the person confirms the lock.
- Our codes are unique on the site, since the URL names no organisation; they
  resolve only for someone in the organisation that minted them.
- (A product's barcode always goes on the article, and a GS1 code's serial
  finds the unit: [ADR-0012](0012-a-products-barcode-names-the-article.md).)
- **Existing codes are aliases** of an article or a unit, unique within a
  organisation: a GTIN/EAN (its check digit decides, kept as 14 digits), a
  manufacturer's QR (its text, or a SHA-256 of it when long), an NFC tag's
  UID.
- The site's `/scan/` reads codes with the phone's camera (the browser's
  BarcodeDetector, or the bundled `@zxing/library`) or NFC, and opens the
  part's card at `/t/<code>`. A code nobody knows yet is offered to be bound
  there.
- Labels print from the browser on A4 sticker sheets: tiny (10 mm), tag
  (20 mm) and article (70 × 37 mm).

## Consequences

- Codes never change meaning: unbinding one of ours makes it blank again, an
  alias is retired, and the movement log keeps what happened.
- A label printed today still works when the part moves to another workspace.
- The hosting needs rewrites for `/t/*` and `/T/*`; without them, the 404 page
  forwards a code to `/t/?c=<code>`.
