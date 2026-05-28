# DESIGN.md — Status

> Snapshot of where the DESIGN.md initiative stands. Updated as the work moves; this file is the canonical "what is happening right now" view alongside `DESIGN-md-mission.md` (the why) and the Notion practical guide (the public framing).

**Last updated:** 2026-05-28
**DRI:** Ben VanderVeen (`ben.vanderveen@customer.io`)
**Linear project:** [Design.md](https://linear.app/customerio/project/designmd-3d70a4786aa1) (Pluma team · status: Idea · priority: High · start 2026-05-11 · target 2026-07-31)
**Notion practical guide:** [DESIGN.md at Customer.io — A Practical Guide](https://www.notion.so/custio/DESIGN-md-at-Customer-io-A-Practical-Guide-35a4302f4c2b80bfba15d593312ded87) (Draft v1.1, last updated 2026-05-13)

## What shipped to the bundle this week

- `DESIGN.md` v0.1 skeleton — 16 sections wired in (Stitch base nine + seven Customer.io extensions), opinionated defaults (desktop-first, dense, no exclamation marks, 14px body, tabular numerals)
- `tokens.md` — full color and typography token reference, component-specific aliases, Journeys workflow tokens, composite text tokens, deprecations
- Per-component AI guidelines for Banner, Breadcrumbs, Button, DataTable, Label, Modal
- `DESIGN-bundle.md` (~18k tokens, plus `.zip`) — single-paste version of all of the above for sharing with collaborators who want to try DESIGN.md against an AI agent without attaching eight separate files

## What's running in parallel

These are adjacent efforts kicked off this week that DESIGN.md needs to slot into, not compete with.

- **Andreas Pappas pilot (2026-05-12).** First external test of `DESIGN-bundle.md`. Use case: the consolidated signup flow Andreas is prototyping in Claude Design. Matt explicitly asked "what's the design.md in this scenario?" Outcome will tell us whether bundle-as-context is enough or whether we need a thinner v0.
- **Shanil's designer-skills workshop (started 2026-05-13).** Goal: a tool-agnostic repository of product designer skills that any agent can call. Affinity-mapping in Miro. Shanil framed DESIGN.md as "one part of the larger system… maybe the orchestration .md file for the others." Ben and Shanil aligned 2026-05-14 that DESIGN.md should dovetail into this rather than be parallel to it.
- **Richard's design principles workshop (scheduled 2026-05-20).** Outputs will feed scaling-design efforts: DESIGN.md, skills, Shiny, automated reviews. Prep asks coming early next week.
- **Richard's "engineering enablement" investigation.** Separate Notion write-up. Open question Richard raised: should the 16 sections live in Pluma rather than in a separate `DESIGN.md`? Tracked in Notion comments; resolution still pending.
- **Pluma MCP / `ai-content-guidelines.md` outputs (Ashley).** Per-component AI guideline files are being generated for every Pluma component. DESIGN.md should point at these, not duplicate them.

## Open scoping pressure

Matt pushed back on 2026-05-11 with: *"What's the tiniest useful scope that a first, limited version of DESIGN.md could solve?"* He listed six candidate capabilities:

1. Excellent copy
2. Tables that don't suck
3. Correct uses of color
4. Solid/good-enough charts
5. Sprinkled-in motion
6. (open)

Ben's response: **Copy, color, motion** are slam-dunks for v0. **Tables and charts** are less certain — charts haven't been overhauled and tables haven't fully been plumatized. v0 should lead with the slam-dunks and explicitly defer the rest. This narrowing is not yet reflected in `DESIGN.md` or the bundle.

## Open questions from the design team (2026-05-12 review)

Tracked in the Notion guide; restating here so they live alongside the file they describe.

1. **Governance** — "Is there a designer approval the way engineers approve a PR?" The boundary between "ship solo" and "needs design sign-off" is not yet named. Maps to §13 (Escape Hatches & Anti-Patterns).
2. **DESIGN.md ↔ Pluma MCP** — when to reach for which? Needs a short decision table (file vs. MCP vs. per-component docs). In progress.
3. **Maintenance** — flagged as an open risk. CI lint on inline values is the mechanical check; the human checks need a named owner.
4. **Principles depth** — Shanil's push: principles need detailed standards, real use cases, explicit anti-patterns. One-liners won't be useful to AI. In progress.
5. **Engineer segmentation** — Bernard/Nelson/Max already guess and loop in design; Jen/Gen follow designs but lack judgment confidence; Paul makes assumptions that surface in QA. DESIGN.md is most valuable for the junior cohort, who also have the least context for knowing when they've gone off-script.

## Signals worth tracking

- **Engineer-driven UIs without FE/design are increasing.** Richard, Akram, Shanil all confirmed seeing this pattern in the 2026-05-14 thread — validates the project premise. Worth a metric to watch over the 90-day window.
- **The framing is "engineer confidence," not just "error prevention."** Jen's reaction and Bernard's ask both point at confidence-as-the-win. That changes what §13 needs to do.
- **Pluma is now public-shareable (2026-05-28).** The site at https://pluma.customer.io/ has the Polaris-shaped IA (Foundations / Components / Patterns / Advanced), a public MCP setup page, versioned packaging, and per-page "Copy for LLMs" export. Patterns section already holds three pages. This collapses the consolidation timeline — Phase 1 of the pitch is substantially in place; Phase 2 (porting the payload) is the next move. Richard's "why not in Pluma" open question is resolved by the site architecture itself.

## Next steps

- [ ] Shape the five payload sections (Voice, Refinement levers, States, Density, Anti-patterns) as Pluma pattern-page candidates — drafts ready to hand to Kevin / Pluma team for porting
- [ ] Pin the porting conversation with Kevin / Pluma team — order of operations for moving payload from `benvanderveen-rgb/design` into `pluma.customer.io/patterns`
- [ ] Open the announce / no-announce question with Matt + EPDL — public Pluma URL is shareable today; what is the timeline on treating it as a Polaris-style external surface?
- [ ] Decide the fate of `DESIGN-bundle.md` — keep as a portable artifact for pre-MCP use cases, or retire once payload lives in Pluma
- [ ] Decide v0 scope explicitly — confirm copy / color / motion as the slam-dunk capabilities; defer tables and charts to v0.1
- [ ] Reconcile with Shanil's designer-skills workshop output — is DESIGN.md the orchestration layer, a sibling, or a subset?
- [ ] Name the "ship solo vs. needs design" boundary for §13
- [ ] Pull the principles exercise forward; feed into both DESIGN.md and Shanil's skills
- [ ] Identify the DRI (currently TBD in the Notion guide; Ben is acting DRI for v0)
- [ ] Pick the first surface overlay (Journeys or Dashboards) once the v0 scope lands
- [ ] Add the inline-hex CI lint to the design-system repo
- [x] Ship the bundle to a first external tester (Andreas, 2026-05-12)
- [x] Hold the design-team review (2026-05-12)
- [x] Move the Linear project from "Pluma.md" naming to "Design.md" — 2026-05-11
- [x] Resolve Richard's "why not in Pluma?" — answered by the public site's IA (Foundations / Components / Patterns / Advanced), 2026-05-28

## Reference

- Linear project: https://linear.app/customerio/project/designmd-3d70a4786aa1
- Notion guide: https://www.notion.so/custio/DESIGN-md-at-Customer-io-A-Practical-Guide-35a4302f4c2b80bfba15d593312ded87
- Pluma design system (public): https://pluma.customer.io/
- Pluma MCP setup (public): https://pluma.customer.io/overview/mcp-server
- Stitch DESIGN.md announcement: https://blog.google/innovation-and-ai/models-and-research/google-labs/stitch-design-md/
- Awesome DESIGN.md examples: https://github.com/VoltAgent/awesome-design-md
