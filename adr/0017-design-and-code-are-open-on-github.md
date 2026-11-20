# ADR-0017: A workspace's design and code are open on GitHub; its business stays in the cloud

Status: **Accepted**

## Context

Maker-5 is open source, and what is made with it should be too. The edge is
where a workspace is designed: its agent and the person draw the hardware
(blocks, pins, wires) and write the software. That work belongs in a public
repository, where others can read, reuse and improve it. Some of a
workspace is not design, though: what the organisation has in stock and
where, what parts cost and who supplies them, who works on it, and the
proposals and chats on the way. That is the organisation's business, and
possibly its IP.

The GitHub organisation **maker-5-platform** hosts the workspaces'
repositories.

## Decision

- **One repository per workspace**, `maker-5-platform/<workspace>`, public
  by default (`GITHUB_ORG`, `REPO_VISIBILITY` in the edge's
  `edge/settings.py`).
- **The edge works with the person's own GitHub login** (the GitHub CLI,
  `gh`, and `git` on that computer). It creates the repository, clones it
  into the workspace's folder and pushes. Credentials are used for that
  repository only (a local credential helper). The cloud holds no GitHub
  secret and never talks to GitHub.
- **What is public**, in the repository:
  - `software/`: the code. The person and the agent write it; the agent
    only by proposals the person confirms, and only under `software/`.
  - `hardware/`, written from the model on every push and never edited
    there:
    - `model.ttl`: the product, blocks, modules, interfaces, pins and wires,
      without stock links (`m5:usesArticle`), quantities, provenance or
      proposals;
    - `wiring.svg`: the hardware canvas;
    - `netlist.md`: every wire, pin to pin;
    - `bom.md`: the parts by name and count.
  - `NOTICE`: the Fritzing parts used, with their authors and licence
    ([ADR-0016](0016-parts-from-the-fritzing-library.md)).
  - `README.md`, `LICENSE` (software: GPL-3.0), `LICENSE-hardware`
    (CERN-OHL-S-2.0), `.gitignore`.
- **What stays in the cloud:** the stock, units, boxes and places, suppliers
  and prices, members, proposals and decisions, chats and the files given
  to them. The cloud keeps only the link: repository, branch, last commit
  and when, on the space's Graph row, written by `/edge/v1/repo`. The
  workspace's Overview and its landing card show it.
- **A push is reviewed and guarded.** The edge writes `hardware/` again,
  stages everything and shows the diff. Before committing, a guard reads
  the staged files and refuses AWS, GitHub, Slack and AI keys, private-key
  blocks, password assignments, the edge's device token, e-mail addresses
  other than example ones, and files that are never published (the edge's
  `cloud.json`, `.env`, keys, the inbox). A finding stops the push and
  nothing is sent. A push behind GitHub asks for a Pull first, which only
  fast-forwards.
- **The agent** reads the repository (`repo_status`, `repo_files`,
  `repo_read`), proposes files (`repo_write`) and proposes a push
  (`repo_push`). Each is confirmed by the person, like any change.

## Consequences

- Making a public repository publishes. The page says so before it makes
  one, and the first push comes only after the person has reviewed it.
- The guard is a net, not a proof. It cannot recognise a private name
  written as prose, so the review before a push still matters.
- `hardware/` is a projection of the model. A change made there by hand is
  overwritten by the next push; the model is where the design is changed.
- A workspace with no edge can still be read on GitHub. Writing to it goes
  through an edge, or through ordinary pull requests on GitHub that the
  edge then pulls.
