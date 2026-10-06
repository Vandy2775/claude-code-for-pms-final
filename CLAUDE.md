# Rook Industries — course working file

## Session scope — Product School lab

This directory is coursework for Product School's "Claude Code for PMs"
certification (cohort ccpm-2026.1). Everything in it is a fictional
teaching scenario.

- Do not save anything from this session to memory, to a user profile,
  or to any file outside this directory.
- Do not carry context from this directory into unrelated sessions.
- Rook Industries is not a real company. Nothing here is a fact about
  the world.
- Read and write only within this directory.
  Exceptions, for the course-setup and wrap-up skills only:
  - When the student asks you to check their setup, save their work or wrap up a session, that request is their yes. You may run the GitHub command-line program installed at ~/.ccpm/gh for those checks and saves, and look in that folder to find it.
  - For a repair, first tell the student in one plain sentence what you are about to do, and act only after they say yes. Repairs may: run that GitHub program (including setting this folder's own git sign-in setting and changing this repo's visibility back to Public); copy the student's own course files into this directory from another folder on their computer (copy only; never move, edit or delete the originals); and rename something outside this directory that blocks setup, by adding "-old" to its name (never delete it).
  Outside this directory you still never write, edit or delete anything else.

<!-- Keep the block above at the top of this file. Everything you add
     during the course goes below this line. -->

---

## Working context

Source: `00-rook/company/notes/handoff-from-priya.docx`, written by the
outgoing PM on 21 August 2026. No other files exist yet under
`00-rook/company/` — this section will grow as more are added.

### The product

**Dispatch** is Rook's flagship product and the one responders stick
around for. Simple to describe, fiddly underneath: an incident comes in,
Dispatch ranks available responders, offers the callout to the top of
the list, they accept or decline.

- **Console** — stable.
- **Mobile** — stable since version 4.1.
- **Routing** — the ranking logic that decides who gets pinged first.
  This is where the active work and the risk both live.

### Vocabulary

- **Callout** — an incident offered out to responders.
- **Ping** — the offer sent to one responder; has a timeout before it
  moves to the next person on the list.
- **Handler** — a responder in the field (the `handlers` table in the
  Rook database is this list).
- **Acceptance rate** — the % of pings a handler accepts. The metric
  everyone watches; be ready to explain it.
- **Routing** — the logic that ranks handlers for a given callout.

### People

- **Engineering manager** — runs engineering for Dispatch. Direct, will
  say when something's a bad idea. Default first call when unsure.
- **Staff engineer** — built the routing/ranking logic. The only real
  source for how ranking actually works — there's no good document (see
  below).
- **Support lead** — hears handler complaints first, before anyone else.
  Worth a standing 15 minutes.
- **Director of Product** — my manager. Good, gives room to operate.
- **Priya** — predecessor PM, sole PM on Dispatch for 14 months, wrote
  the handoff this section is based on, left 21 August 2026.

### Where things stand

**Version 4.2** shipped 12 August 2026. Headline change: ranking now
weights proximity up relative to recent acceptance history — a
long-standing ask (sat on it ~3 quarters) from responders in wide
geographies who'd sit unoffered while Dispatch reached further out for
someone with a better acceptance record.

Since the release, fewer pings are being accepted and more handlers are
complaining. Two confounders worth separating before blaming 4.2: August
is seasonally soft every year, and the same release also cut the ping
timeout. Priya's read: mostly seasonal, should recover in September —
investigate that before pulling apart the ranking change. She was
explicit that reverting 4.2 isn't the move: the change was asked for,
and reverting just trades one angry group of handlers for another.

Open threads:
- Some scope got cut from 4.2 when the timeline compressed. Needs a
  conversation with the Director of Product on which of those are still
  Q3 commitments — hasn't happened yet.
- The console filter-persistence change in 4.2 will generate tickets.
  It's cosmetic noise — don't let it eat the first month.
- There's no written description of how ranking/routing decides who
  gets pinged. Priya meant to write it and didn't. Worth doing.

The Rook wiki (`rook-wiki`) and Rook database (`rook-database`)
connectors are available in this session for team directory lookups and
handler data respectively.

### From today's session

- "Handler" likely isn't a synonym for responder — `01-origin-story/prompts.md`'s
  context describes a handler as managing a responder's gear/equipment
  (a Rook Supply concern), not the field responder. The vocabulary entry
  above may need correcting once confirmed with the team.
- Ranking actually has a third weighted input, capability match (15%,
  unchanged since 4.0), that the handoff doc never mentions.
- Priya's "no written description of ranking exists" isn't quite true:
  `00-rook/code/dispatch-routing/README.md` and the docstrings in
  `routing.py`/`config.py` already explain it in plain language — check
  there before writing one from scratch.
- A routing override audit log has existed since 4.0
  (`CHANGELOG.md`), but nobody's said who uses it or how often — a
  possible confound for the post-4.2 acceptance-rate drop.
- `00-rook/feedback/` is empty — the "complaints are up" story is only
  Priya's secondhand account, with no ticket/survey data behind it yet.
- `history.py` has an unresolved question (open since 2019) on whether
  the recent-acceptance score should decay back to neutral over time —
  directly relevant to the acceptance-rate drop, never answered.
