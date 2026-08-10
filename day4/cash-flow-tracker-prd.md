# Cash-Flow Tracker — Sunrise Bakery

> This is the output of the `idea-refine` skill, run on the bakery brief.
> Save it in your project as `docs/ideas/cash-flow-tracker.md`.

## Problem Statement

How might we let the owner of a small bakery see where they stand financially at
the end of a day, without a calculator, a spreadsheet, or any training?

## Recommended Direction

A single-page **Cash-Flow Tracker** that runs on the owner's own laptop. Money
coming in and money going out are recorded in the same short form; the balance
updates as they go.

The direction we chose is deliberately the plainest one. Two richer options were
considered and rejected: a full accounting tool with categories, reports and
monthly closing, and a phone-first app that could be used at the counter. Both
are better products; neither can be built and understood in one session, and
both hide the thing we actually want visible — a small stack that a beginner can
hold in their head all at once.

Everything runs locally. No accounts, no internet dependency once installed, and
no AI inside the finished product. The AI is in how it gets built, not in what
gets shipped.

## Key Assumptions to Validate

- [ ] One entity is enough — a bakery genuinely doesn't need separate customers,
      invoices or suppliers to answer "where am I today?"
- [ ] A fixed category list beats letting the owner create their own — fewer
      decisions, and nothing to maintain
- [ ] A running balance is the one number that matters; everything else is
      decoration

## MVP Scope

**In scope**

- One `Transaction` record: date, description, category, type (in or out), amount
- Add a transaction from a form on the page
- See every transaction in a table, newest first
- A running balance, always visible
- Delete a transaction that was entered by mistake

**Fixed categories:** Sales, Supplies, Rent, Utilities, Transport, Other

**Amounts are whole rupees** — no decimals, no currency symbol. Money in and
money out are both entered as positive numbers; the in/out toggle decides the
direction.

**Stack:** NestJS · SQLite via TypeORM · plain HTML, CSS and JavaScript

## Not Doing (and Why)

- **Login or user accounts** — one person, one laptop. Sessions and passwords are
  a whole separate subject and would consume the session.
- **Editing a transaction** — delete and re-enter is enough. Editing doubles the
  API surface for a rare case.
- **Managing categories** — a fixed list removes a second entity, a second set of
  screens, and a decision the owner doesn't want to make.
- **Charts or graphs** — a charting library is an external dependency and a new
  vocabulary. Plain bars made of CSS say the same thing.
- **Reports, exports, date ranges** — real needs eventually, none of them needed
  to answer today's question.
- **Deployment** — it runs on the machine it was built on. Putting it online is a
  different lesson.

## Open Questions

- Does the owner want yesterday's balance carried forward, or does each day start
  from zero? *(Assumed: running total, never reset.)*
- Should money out be entered as a negative number or chosen with a toggle?
  *(Decided: a toggle. Fewer mistakes than typing a minus sign, and it keeps
  every stored amount positive — which makes the balance arithmetic obvious.)*
