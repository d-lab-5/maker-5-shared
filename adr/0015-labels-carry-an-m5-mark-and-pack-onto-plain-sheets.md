# ADR-0015: Labels carry an M5 mark, and a print queue packs them onto plain sheets

Status: **Accepted**

## Context

The labels of [ADR-0009](0009-codes-and-tags.md) print one layout per sheet, in the
grid of die-cut label paper. The workshop's paper is plain A4 sticker sheets,
cut by hand. Printing a few labels at a time wastes most of a sheet, and a box
label and twenty tiny ones can't share it.

The house has other QR codes too: DigitalHome.Cloud's codes control the smart
home. Looking at a label, it should be clear which system it belongs to.

## Decision

- **Every label prints its code as `M5 K7Q2M9`.** The mark is printed text
  only; the QR still holds `HTTPS://<host>/T/K7Q2M9`, so labels already
  printed keep working. A code typed or scanned with its mark (`M5 K7Q2M9`,
  `M5-K7Q2M9`, `M5K7Q2M9`) is the same code.
- **A print queue** collects labels of any size, from articles, units, boxes
  and blank codes, until a sheet is worth printing:
  - **Where it lives:** in the browser, per organisation. It is a desk's queue,
    not a team's.
  - **Packing:** "Print queue" on Materials packs the labels onto as few A4
    sheets as hold them (`lib/labelPack.ts`): first fit, tallest first, in
    shelves, with small labels stacked beside large ones. It keeps a 7 mm
    printer margin, leaves 2 mm between labels for the cut, and can draw
    dashed cut guides.
  - **Part-used sheets:** after printing, the part of the last sheet that is
    left is remembered. The next print skips the used top when the same sheet
    is put back in the printer.
- The die-cut layouts stay, for label paper with a grid.

## Consequences

- Fewer sheets: for example, 5 box labels, 7 tags and 60 tiny labels fit on
  one sheet.
- The queue isn't shared between computers. Printing from the phone still
  means the one-layout sheet.
