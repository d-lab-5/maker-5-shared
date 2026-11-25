# ADR-0024: Supplier documents: read on the edge, duplicates found, kept on the parts

Status: **Accepted**

## Context

The order e-mail ([ADR-0023](0023-orders-expected-until-scanned.md)) made
Amazon's orders expected parts. But a shop sends more than order
confirmations: shipping notices, invoices, delivery notes, service reports.
Other shops, such as ELV, put the table in an attached PDF rather than in
the e-mail. And the same part comes back: ordered again, returned from a
repair, invoiced after it arrived. Each of these made a second article, and
the invoice or the service report was lost in a mailbox, away from the part
it was about.

## Decision

- **One door for every supplier document.** An e-mail (`.eml`), a PDF or
  pasted text is attached in the edge's chat with 📎, or dropped on the
  Intake tab's **Supplier document**. The edge reads it:
  - the shop (from the sender's domain);
  - its kind (`order`, `shipment`, `invoice`, `service` or `other`), its
    number and date;
  - its lines (title, the shop's article number, quantity, price);
  - its attachments (PDFs and office files, each with its sha256).
- **A reader per shop:** Amazon (the e-mail's HTML), ELV (the table in the
  attached PDF). A shop without a reader gives its text, and the edge's
  agent writes the lines itself. That is how a new supplier works before it
  has a reader.
- **Duplicates are found before anything is written.** Each line is checked
  against the stock and against what is expected:
  - **sure matches:** the shop's article number on an article
    (`supplierRefs`), the maker's part number (`mpn`), or the article's name
    in the title;
  - **candidates:** up to three names that look alike (shared words, the
    brand counting twice), shown with their score; they are never merged
    on their own;
  - **known:** a line read before (shop, number and article number).
- **Each line is expected, linked or skipped.**
  - **expect:** an order line, as in ADR-0023. When the article is known,
    the line names it, so no second article is made.
  - **link:** the part is ours already, as with a service report or an
    invoice for something checked in. The shop's number goes onto the
    article, and so do the documents.
  - **skip:** shipping costs, services, what isn't a part.

  An order, a shipping notice or an invoice proposes **expect**; a service
  report or another document proposes **link** when the part is found.
  The person changes either.
- **Documents are kept on the parts, privately.** Each attachment is
  uploaded once to the organisation's private storage in the cloud
  (`docs/<organisation>/<file>`), and the same content (sha256) is never
  uploaded twice. It is listed under **Documents** on every article it
  concerns. On an expected line it waits until check-in, which moves it
  onto the article. The documents carry the buyer's address and customer
  number: they are never written to a workspace's public repository
  ([ADR-0017](0017-design-and-code-are-open-on-github.md)), and the agent
  doesn't quote them. The e-mail's own text is not kept, not even on the
  edge once applied.
- **The chat proposes, the person confirms.** The agent reads the document
  (`supplier_read`) and proposes the import (`supplier_apply`) as one
  proposal; the person's click runs it. This amends ADR-0023, where only
  the Intake tab imported. Booking stock is still only the admin's check-in
  ([ADR-0022](0022-intake-scanned-at-the-shelf.md)).
- **Anyone in the organisation** adds and removes documents, as with a box's
  files ([ADR-0011](0011-boxes-hold-parts-and-their-artifacts.md)).

## Consequences

- The flow is drawn under ADR-0023: *From the order to the shelf*, now with
  the link to a part we have.

- An article shows its invoices, delivery notes and service reports, on the
  phone and on the web.
- Ordering the same part again, or getting it back from a repair, finds the
  article instead of making a second one.
- The edge needs `pdftotext` (poppler) to read PDFs; without it, a PDF's
  lines aren't found, but it is still kept on the parts.
- Not now: a forwarding address that reads e-mails by itself, OCR for
  scanned PDFs, readers for Reichelt, Mouser and AliExpress (the agent
  covers them meanwhile).
