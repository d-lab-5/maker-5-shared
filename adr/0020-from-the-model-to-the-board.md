# ADR-0020: From the model to the board: a pin map and a deployment the model records

Status: **Accepted**

## Context

A workspace's hardware is drawn on the edge's canvas and its software is
written in its repository ([ADR-0017](0017-design-and-code-are-open-on-github.md)).
Two things fell between them. Code named its pins by hand, so the wiring and
the firmware could disagree without anyone noticing. And getting the code
onto the board was done outside Maker-5, so nobody could say which version
ran where.

## Decision

- **A pin map from the model.** On every push (and before a deployment) the
  edge writes `software/generated/pins.py` and `pins.json`: one class per
  board that software runs on (else per wired block), with a constant per
  wired GPIO, named after what it goes to, and comments for the other wired
  pins. A GPIO number comes from the pin's name (`GPIO17`, `GPIO 17/TXD`,
  `IO5`). The folder is read-only on the page and to the agent.
- **The agent imports it.** Its instructions say to take pins from
  `pins.py`, never to write a GPIO number, and to wire a missing pin on the
  canvas first. The review before a push lists GPIO numbers the code uses
  that the wiring does not have.
- **Deploying from the edge.** A target is a block of the model and how to
  reach it: a host, user, port and folder over SSH, or this computer when
  the edge runs on the board. It has an optional restart command and log
  command. Deploying copies `software/` there with `rsync --delete`, runs
  the restart command and reads the log back.
  - SSH runs in batch mode with the person's own key or agent; no password is
    asked or stored.
  - A folder is only written if it is new, empty, or marked by an earlier
    deployment; a home or system folder is refused outright.
  - Targets are kept in the workspace's folder on that edge only
    (`deploy.json`). Hosts and users are never in the repository or the
    cloud.
  - The person sets targets on the Code tab. The agent can list them and
    propose a deployment, which the person confirms; it can never add or
    change a target.
- **The model records it.** A successful deployment sets
  `m5:deployedCommit` (a full commit id) and `m5:deployedAt` on the board's
  block (ontology 0.6.0). The edge also notes when the deployed code had
  changes not yet pushed.

## Consequences

- The wiring and the firmware can no longer drift apart silently: a rename
  on the canvas changes a constant, and a stale one fails at import.
- Which commit runs on which board is part of the design and syncs to the
  cloud with the rest of the model. Since the public `hardware/model.ttl`
  carries it, the commit id is public; the host is not.
- An edge that is not on the board's network cannot deploy to it. That is
  deliberate: the cloud never reaches into a workshop.
- Deploying a commit with unpushed changes is allowed, for trying things on
  the bench, but it is marked, because that commit does not hold what runs.
