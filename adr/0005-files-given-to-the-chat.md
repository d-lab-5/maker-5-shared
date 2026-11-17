# ADR-0005: Files are given to the chat, never applied

Status: **Accepted**

## Context

Much of what a hardware product is lives in files: KiCad schematics and
boards, STEP exports, datasheets, BOMs. A person wants to hand one to the
chat and have the model filled from it, without a parser for every format
that writes to the model on its own.

## Decision

A file dropped on the Files tab or on the chat is kept on the edge
(`maker_5_edge/inbox.py`): in the open product's `files/` folder, or in a
shared `inbox/` when none is open. Its kind is detected from its content,
not its name (`kicad`, `step`, `csv`, `xlsx`, `turtle`, `json`, `pdf`,
`text`), with a short note of what was found.

The chat reads the file in slices and proposes the blocks, modules and
interfaces it finds, one tool call at a time, each confirmed by the person.
A file never changes the model by itself.

## Consequences

- Any format the chat can read is supported, without code for it.
- Files stay on the person's computer; nothing here sends them anywhere.
