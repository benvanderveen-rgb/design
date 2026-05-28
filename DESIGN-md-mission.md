# DESIGN.md at Customer.io

A machine-readable spec for how Customer.io UI looks, behaves, and sounds — written for the AI agents we already use.

> This file is the **why**. For current status, the pilot, parallel design-team efforts, and open scoping questions, see [`STATUS.md`](./STATUS.md). For the **how**, read `DESIGN.md` itself.

## Why this exists

Engineers, designers, and PMs at Customer.io already prompt Cursor, Claude, and Stitch to generate product UI every day. Without guardrails, each agent invents its own version of Customer.io — wrong components, wrong density, marketing-page voice. Off-brand at best, broken at worst.

DESIGN.md fixes that at the file level. One document, committed to the Fly repo next to `AGENTS.md` and `CLAUDE.md`, teaches every agent what Customer.io UI looks, behaves, and sounds like. Cursor, Claude, and Stitch read it before they build. What comes back is closer to something we'd ship.

## What we're really building

A self-service engineering kit — not a style guide for humans, not a replacement for the design system. A legibility layer that lets engineers compose features end-to-end with AI assistance, on patterns we've already approved.

The bar isn't "indistinguishable from a designer's work." The bar is decent: features that ship, hold up to use, and don't require designers to retype the basics. When engineers can clear that bar alone, designers stop being the bottleneck on layout and start owning what only they can — research, new domain affordances, the patterns that don't yet exist.

That trade is the point. DESIGN.md is the artifact; the outcome is shifting design's center of gravity from feature factory to system innovation.

## Does this replace product designers, or their role?

No. It replaces the parts of design work that nobody became a designer to do.

The repetitive layout pass on a feature pattern we've shipped fifty times. The Slack message reading "can you tweak the copy on this button." The Figma file that's really a translation of a PRD into something an engineer can pick up. None of that is the work designers got into the field for, and none of it is what makes Customer.io's product distinctive.

What designers get back is the work that needed them all along: the patterns we don't have yet, time spent in research, ownership of the surfaces where craft is visible — onboarding, the journey canvas, the moments customers feel before they can name. That work has been losing every week to feature-factory urgency. DESIGN.md is the lever that swings the balance back.

The role doesn't shrink. It moves up.

## What success looks like

A year from now, the median Customer.io feature is composed by an engineer with Claude Code and reviewed by a designer in minutes rather than co-authored over days. Designers spend their time on research, new patterns, and surfaces that need invention — not on margins, copy, and which combobox.

In ninety days, more concretely:

- Every Fly repo PR references DESIGN.md as a source of truth
- Engineers can self-serve a defined set of feature types end-to-end without design review
- We can name the boundary between "ship solo" and "needs design" without arguing about it
- Time spent by designers on rote feature work is measurably down

## What this isn't

DESIGN.md is not the design system. The design system is **Pluma** ([pluma.customer.io](https://pluma.customer.io/)) — the component library, tokens, and pattern documentation we already maintain. DESIGN.md is the rules layer that makes Pluma legible to AI agents.

It doesn't replace design review for new patterns, customer-facing changes, or anything touching the journey canvas, onboarding flows, or Liquid editing. The escalation list lives in §13 of the file itself.

It isn't marketing. Customer.io's marketing surfaces have their own voice, palette, and rhythm. DESIGN.md governs the product — the tool, not the brochure.

## How it gets built

A single token source of truth — codebase, Figma Variables, or a neutral JSON layer between the two — feeds the auto-generated sections of the file. The hand-authored sections (voice, motion, density, escape hatches) sit on top. CI lints inline values to prevent drift. Surface overlays under `apps/journeys/`, `apps/parcel/`, and so on extend the root for surface-specific rules.

Owned by the design systems lead. Maintained by anyone touching tokens or shipping new patterns. Read by everyone who prompts an agent.
