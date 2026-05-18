# Prompt cookbook

Starter prompts for asking Claude Design (or Cursor, Stitch, Copilot) to generate Customer.io UI. Pick the recipe that matches what you're building, fill in the bracketed parts, paste into the tool.

You do not need to know which section of DESIGN.md applies — every recipe already points at the right files. The agent does the routing.

## How to use a recipe

1. Find the task you're prompting for in the list below.
2. Copy the prompt template under it.
3. Replace the `[bracketed]` parts with your specifics.
4. Paste into the tool.
5. Compare the output to the "what good looks like" checklist beneath the recipe. If something is off, the spec or the recipe is the gap — flag it in `STATUS.md` or `pressure-check.md`.

## 0 — Smoke test (run this first, only once)

Use to confirm Claude Design has actually read the repo.

**Prompt:**

```
Before generating any UI, tell me what files you would read first
to follow the Customer.io design spec. List them in the order you
would read them.
```

**What good looks like:**

- Names `AGENTS.md` first.
- Then `DESIGN.md`.
- Then the relevant per-component file based on what comes next.
- Mentions `tokens.md` for color and spacing values.

If the response doesn't name those files, the codebase integration isn't reading the repo. Stop and reconnect before running anything else.

---

## 1 — Confirm a destructive action

Use when the user is about to do something irreversible: delete, archive, purge, revoke access.

**Prompt:**

```
Design a destructive-confirmation modal for [action, e.g., "deleting
a segment named 'Active US Users' that is used by 3 live campaigns"].

The user should have to [type the segment name | check a confirmation
box | type DELETE] before the primary action is enabled.

Follow the destructive-confirmation pattern.
```

**What good looks like:**

- Action-oriented title (e.g., "Delete segment 'Active US Users'"), never "Are you sure?"
- Close button hidden.
- Body names the consequence (which campaigns break, what data is lost).
- Text field or checkbox required to enable the primary action.
- Primary action uses `isDanger` and a specific verb ("Delete segment", not "Confirm").

**Spec sections this recipe uses:** `modal.ai-guidelines.md` (destructive-confirmation pattern), `DESIGN.md` Voice and Components.

---

## 2 — Celebrate a feature launch (feature moment)

Use for onboarding moments, launch announcements, and "you've unlocked this" surfaces.

**Prompt:**

```
Design a feature-moment modal celebrating [feature, e.g., "the launch
of multivariate testing for campaigns"]. Include a large branded
illustration.

The audience is [existing customers | new customers | enterprise admins].
The tone should be [calm | warm | dense] — not marketing.

Tell me which media slot and panel proportion you chose and why.
```

**What good looks like:**

- Names a sanctioned media slot: `top`, `leading`, `trailing`, or `bleed`.
- Names a sanctioned panel proportion: `50/50`, `40/60`, or `60/40`.
- No exclamation marks, no "Awesome" or "Just" or "Easy."
- Single primary CTA with a specific verb.
- Cites `modal.ai-guidelines.md#refinement`.

**Spec sections this recipe uses:** `modal.ai-guidelines.md` Refinement (media placement, panel proportion), DESIGN.md Voice.

---

## 3 — Empty state

Use when a list, table, or page has zero content yet.

**Prompt:**

```
Design the empty state for [surface, e.g., "a segments list page when
the user has zero segments yet"].

The user is [new to this feature | experienced but just cleared everything].
What is the single action that moves them forward?
```

**What good looks like:**

- Icon + one-line "what this is" + single primary CTA with a verb.
- Never "Nothing to show" or "No segments yet" as the title.
- CTA copy is action-oriented ("Create your first segment").
- Background uses `color-surface-subtle`, padding uses `space-400`.

**Spec sections this recipe uses:** `DESIGN.md` ## States, `surfaces.ai-guidelines.md` Padding scale.

---

## 4 — Error state with recovery

Use when something failed and the user needs to recover.

**Prompt:**

```
Design the error state for [scenario, e.g., "a CSV import that
partially failed — 8,556 of 12,403 rows succeeded"].

What is the user's recovery path? What information do they need to
see to act?
```

**What good looks like:**

- Leads with what succeeded if it's a partial failure ("8,556 of 12,403 rows imported"), not "Import failed."
- Names the gap ("3,847 rows had invalid email addresses").
- Primary action is a recovery path ("Re-upload corrected file"), never "Dismiss" or "OK."
- Non-trivial recoveries get three actions: dismiss, view detail, fix.
- No "Sorry" or "Oops."

**Spec sections this recipe uses:** `DESIGN.md` ## States (Error section), `DESIGN.md` Voice.

---

## 5 — Form

Use for any data entry: creating a campaign, editing settings, configuring a workflow node.

**Prompt:**

```
Design a form for [task, e.g., "creating a new email campaign — title,
audience segment, sending schedule, content"].

Density should match [data-heavy review | standard editing | onboarding].

Tell me the field gap and field-group gap you chose.
```

**What good looks like:**

- Every input is a Pluma primitive: `PlumaInput`, `PlumaCombobox`, `PlumaCheckbox`, etc. No native `<select>` if there are more than 10 options.
- Density named at the form root, not per-field.
- Save bar at the bottom uses `PlumaButtonGroup`.
- Form validation appears inline, not in a banner.
- Cites `DESIGN.md` Density and the relevant component files.

**Spec sections this recipe uses:** `DESIGN.md` Components and Density, per-component files for each input type.

---

## 6 — Data table

Use for any tabular display: campaign list, segment list, audit log.

**Prompt:**

```
Design a data table for [content, e.g., "the campaign list page —
columns for name, status, audience, sent date, open rate, click rate"].

Expected size: [small (<50 rows) | medium (50–500) | large (>500)].
Will the user [scan | sort | filter | export]?
```

**What good looks like:**

- Uses `PlumaDataTable`, never a custom div-based table.
- Strips features the user doesn't need.
- Required caption set (hidden, for assistive tech).
- Column headers in sentence case, full words.
- Right-aligned numbers, tabular numerals.
- Empty state with title, description, icon, action.
- Loading state shows skeleton rows.

**Spec sections this recipe uses:** `data-table.ai-guidelines.md`, `DESIGN.md` Density.

---

## 7 — Settings page

Use for any preferences or configuration surface.

**Prompt:**

```
Design a settings page for [topic, e.g., "notification preferences —
email frequency, push opt-in, quiet hours"].

The user is [new to this | a power user who lives here].
```

**What good looks like:**

- Uses Box-based surface composition, not a custom div layout.
- Settings grouped under headings; surface uses `padding="space-300"`.
- Inline help via `PlumaBanner type="inline"` if needed, never a tooltip-only explanation.
- Save behavior named: auto-save, explicit save bar, or per-field commit.
- Cites `surfaces.ai-guidelines.md`.

**Spec sections this recipe uses:** `surfaces.ai-guidelines.md`, `DESIGN.md` Components, `banner.ai-guidelines.md` for inline help.

---

## 8 — System notice / banner

Use for page-level information: degraded service, maintenance, feature announcement.

**Prompt:**

```
Design a banner for [scenario, e.g., "a sending limit warning when
the account is at 90% of monthly send quota"].

How urgent is this? [informational | caution | error | feature].
Should it dismiss, persist, or block?
```

**What good looks like:**

- Variant matches semantic meaning (`caution` for warnings, `error` for blocking issues), never picked for color alone.
- Type matches scope: `inline` for contextual, `alert` for page-level, `announcement` for app-wide.
- Title is sentence case, few strong words; description carries the detail.
- Single primary action, with dismiss only alongside it.
- Cites `banner.ai-guidelines.md`.

**Spec sections this recipe uses:** `banner.ai-guidelines.md`, `DESIGN.md` Voice.

---

## 9 — UX copy review

Use when you have text — button labels, error messages, empty-state copy — and want it spec-aligned.

**Prompt:**

```
Review this UX copy against DESIGN.md voice rules. Flag anything
that violates them and suggest a replacement.

---
[paste the copy]
---
```

**What good looks like:**

- Catches: exclamation marks, "users," "click here," marketing words ("just," "simply," "easy," "awesome"), apologetic framing ("sorry," "oops"), title-case where sentence case belongs, vague verbs ("OK," "Submit").
- Suggests specific replacements, not generic guidance.
- Says nothing if the copy is already clean — no false flags.

**Spec sections this recipe uses:** `DESIGN.md` Voice and Anti-patterns.

---

## 10 — Scaffold from scratch (engineer mode)

Use when you're building a full page or feature end-to-end and want a starting structure.

**Prompt:**

```
I'm building [feature, e.g., "a campaign performance dashboard"]
end-to-end. I need to ship this without designer involvement.

Walk me through:
1. The page structure (header, surfaces, content blocks).
2. The components I need from Pluma.
3. The tokens for color and spacing.
4. The five states (loading, empty, error, success, permission-denied).
5. Any refinements (padding, density, media placement) and which are
   solo-ship versus design-review per the bounds tables.
```

**What good looks like:**

- Names every component by its Pluma name (`PlumaDataTable`, `PlumaBanner`, etc.).
- All five states present and named.
- Tokens cited for every color and spacing choice.
- Self-flags anything that crosses the governance bar.
- Tells you when you should stop and pull in a designer.

**Spec sections this recipe uses:** all of DESIGN.md, every relevant per-component file. This is the engineer-confidence test — see `AGENTS.md` six-check pre-flight.

---

## When no recipe matches

If you're prompting for something not in this cookbook:

1. Describe what you're building plainly. The agent's job (per `AGENTS.md`) is to figure out which spec files apply.
2. Add this single line at the end of your prompt: *"Follow the DESIGN.md spec and cite the files you used."*
3. If the output is off, add the recipe to this cookbook so the next person prompting the same thing has a head start.

The cookbook grows by use. Every successful prompt is a candidate recipe.
