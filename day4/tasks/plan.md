# Implementation Plan: Cash-Flow Tracker — Sunrise Bakery

> Produced by `planning-and-task-breakdown` from
> `docs/ideas/cash-flow-tracker.md`. Sliced vertically: every task ends with
> something you can open in a browser and use.

## Overview

A single-page tool that records money coming in and going out for a small
bakery, stores it in a local database, and shows a running balance. NestJS
serves both the API and the static page; SQLite holds the data; the frontend is
plain HTML, CSS and JavaScript with no build step. One user, one machine, no
accounts.

## Architecture Decisions

- **NestJS serves the frontend too.** Static files come out of `/public` on the
  same port as the API. One server, one URL, and no CORS to explain.
- **`synchronize: true` on TypeORM.** The table is created from the entity at
  startup. Migrations are the correct production answer and the wrong answer for
  a 90-minute build — they add a vocabulary nobody needs today.
- **Categories are a fixed list in code, not a table.** The PRD rules out
  category management; keeping them as a constant avoids a second entity, a
  second set of endpoints, and a second screen.
- **`type` is `'in' | 'out'`, and `amount` is always positive.** From the PRD's
  open question — a toggle produces fewer input errors than expecting someone to
  type a minus sign, and it keeps the arithmetic obvious.
- **Amounts are whole rupees, stored as an `integer`, and coerced to a number at
  every boundary.** This closes off the most likely bug in the whole build. A
  browser input returns `"5000"`, a *string*, even when the field is
  `type="number"` — so `5000 + 1200` silently becomes `"50001200"` instead of
  `6200`, and the balance is nonsense with no error anywhere. Three defences,
  all cheap: the page sends `Number(value)`, the API rejects anything that isn't
  a finite number, and the column is `integer` so the database cannot quietly
  store text. Choosing whole rupees also removes floating-point drift entirely —
  no `0.1 + 0.2` surprises, and no cents-conversion code. *Trade-off: amounts
  like 1200.50 can't be entered. Acceptable for daily cash in a bakery, and it
  buys a whole class of bug going away.*
- **The balance is calculated on the server, in `GET /summary`.** One source of
  truth. The page displays what it is told and never does maths itself.
- **No tests are written, and no git.** Per the project `AGENTS.md`:
  verification is done by running the app and using it, and there is no version
  control today. Both are the right call for a professional team and the wrong
  one for a 90-minute session with a room that has never used either. The
  `incremental-implementation` skill will want to commit at the end of every
  task — the project rules override it. NestJS scaffolds a default test, which
  is harmless if anyone runs it.

## Dependency Graph

```
Task 1 (scaffold)
   └─→ Task 2 (add a transaction)
          └─→ Task 3 (list + balance)
                 ├─→ Task 4 (delete)      [stretch]
                 └─→ Task 5 (summary bars) [stretch]
```

Tasks 4 and 5 are independent of each other and may be done in either order, or
skipped entirely.

---

## Task List

### Phase 1: Foundation

## Task 1: Scaffold the project and serve a page

**Description:** Create the NestJS project, connect TypeORM to a local SQLite
file, and serve static files from `/public` so that visiting `localhost:3000`
returns a real page. No features yet — this task exists to prove the whole
skeleton runs before anything is built on it.

**Acceptance criteria:**
- [ ] `npm run start:dev` starts without errors
- [ ] `http://localhost:3000` shows a page containing the words "Cash-Flow Tracker"
- [ ] A `cashflow.sqlite` file appears in the project folder

**Verification:**
- [ ] Build succeeds: `npm run build`
- [ ] Manual check: open `localhost:3000` and see the heading

**Dependencies:** None

**Files likely touched:**
- `src/app.module.ts`
- `src/main.ts`
- `public/index.html`
- `package.json`

**Estimated scope:** M — 3-5 files

---

### Checkpoint: Foundation
- [ ] Dev server runs with no errors in the terminal
- [ ] The page loads in a browser
- [ ] The SQLite file exists
- [ ] **Stop here until every machine in the room passes** — everything below
      depends on this

---

### Phase 2: Core Features

## Task 2: Record a transaction

**Description:** The first vertical slice — the entity, the endpoint that saves
it, and the form that calls the endpoint. After this task the tool does its
primary job: money can be recorded and it survives a restart.

**Acceptance criteria:**
- [ ] A `Transaction` entity exists with `id`, `date`, `description`,
      `category`, `type` and `amount`
- [ ] `amount` is declared as an **integer** column — not text
- [ ] The page converts the amount with `Number(...)` before sending it, so the
      API receives `5000` and never `"5000"`
- [ ] `POST /transactions` **rejects** an amount that is not a positive whole
      number, with a clear message
- [ ] The page has a form with all five fields; category is a dropdown of the
      fixed list; type is an in/out toggle
- [ ] Submitting the form clears it and confirms the save

**Verification:**
- [ ] Build succeeds: `npm run build`
- [ ] Manual check: add "Morning bread sales / Sales / in / 5000", then restart
      the server — the record is still in `cashflow.sqlite`
- [ ] **Type check:** ask the agent —
      `show me the saved amount and tell me whether it is a number or a string`.
      It must be a number. This is the single most valuable check in the build.
- [ ] **Rejection check:** try submitting `abc` as the amount — it should be
      refused, not saved

**Dependencies:** Task 1

**Files likely touched:**
- `src/transactions/transaction.entity.ts`
- `src/transactions/transactions.controller.ts`
- `src/transactions/transactions.service.ts`
- `src/transactions/transactions.module.ts`
- `public/index.html`, `public/app.js`

**Estimated scope:** M — 3-5 files

---

## Task 3: See the money and the balance

**Description:** The second vertical slice — read the data back and show what it
means. A list endpoint, a summary endpoint that does the arithmetic server-side,
a table on the page, and the running balance displayed prominently.

**Acceptance criteria:**
- [ ] `GET /transactions` returns all records, newest first
- [ ] `GET /summary` returns `totalIn`, `totalOut` and `balance`
- [ ] The page shows a table of every transaction
- [ ] The balance is visible at the top and updates after each new entry

**Verification:**
- [ ] Build succeeds: `npm run build`
- [ ] Manual check: enter the three test transactions — 5000 in, 1200 out,
      800 out — and confirm the balance reads **3000**

**Dependencies:** Task 2

**Files likely touched:**
- `src/transactions/transactions.controller.ts`
- `src/transactions/transactions.service.ts`
- `public/index.html`, `public/app.js`, `public/style.css`

**Estimated scope:** M — 3-5 files

---

### Checkpoint: Core Features
- [ ] Money can be recorded and read back
- [ ] The balance is arithmetically correct against the three-transaction test
- [ ] The data survives a server restart
- [ ] **This is a complete, useful tool.** Everything below is optional.

---

### Phase 3: Polish *(stretch — skip if the room is behind)*

## Task 4: Delete a transaction

**Description:** Let a mistyped entry be removed. The PRD deliberately excludes
editing, so delete-and-re-enter is the correction path.

**Acceptance criteria:**
- [ ] `DELETE /transactions/:id` removes the record
- [ ] Each table row has a delete button
- [ ] The balance updates immediately after a deletion

**Verification:**
- [ ] Build succeeds: `npm run build`
- [ ] Manual check: delete the 800 expense — the balance moves from 3000 to 3800

**Dependencies:** Task 3

**Files likely touched:**
- `src/transactions/transactions.controller.ts`
- `src/transactions/transactions.service.ts`
- `public/app.js`

**Estimated scope:** S — 1-2 files

---

## Task 5: Show in-vs-out as bars

**Description:** A visual comparison of money in against money out, drawn with
plain CSS. No charting library — the PRD rules one out.

**Acceptance criteria:**
- [ ] Two bars appear, sized in proportion to `totalIn` and `totalOut`
- [ ] Each bar is labelled with its amount
- [ ] The bars are made from styled `div` elements only

**Verification:**
- [ ] Build succeeds: `npm run build`
- [ ] Manual check: with 5000 in and 2000 out, the "in" bar is visibly around
      two and a half times the "out" bar

**Dependencies:** Task 3

**Files likely touched:**
- `public/index.html`, `public/style.css`, `public/app.js`

**Estimated scope:** S — 1-2 files

---

### Checkpoint: Complete
- [ ] Every acceptance criterion in Tasks 1-3 is met
- [ ] The three-transaction test gives a balance of 3000
- [ ] Nothing in the PRD's Not Doing list has crept in
- [ ] The app runs from a cold start: `npm run start:dev`

---

## Risks and Mitigations

| Risk | Impact | Mitigation |
|---|---|---|
| Scaffold takes far longer than 14 min on older laptops | **High** — everything is blocked behind it | Dry-run on the slowest machine available; keep a pre-scaffolded folder ready to hand out |
| Free-tier rate limit during Task 2, the heaviest slice | Med | Everyone made a spare key on Day 2 — switch with `/connect` |
| Agent reaches for a framework or a chart library | Med | It is excluded in `AGENTS.md`; point at the rule and ask it to redo |
| Balance is wrong because the amount arrived as text — `5000 + 1200` giving `"50001200"` | **High** — silent, and it makes the whole tool useless | **Designed out in Task 2:** `Number()` on the page, validation at the API, `integer` column. Backed by the type check in Task 2 and the three-transaction test in Task 3 |
| A pair finishes Task 3 with 20 minutes to spare | Low | Tasks 4 and 5 exist for exactly this |

## Open Questions

- Does the date default to today, or must it be typed every time?
  *(Assumed: defaults to today — it removes a field from the common case.)*
- Should the balance show a currency symbol?
  *(Assumed: no — it avoids a formatting discussion that teaches nothing.)*

## Parallelization Opportunities

Not applicable to this session — everyone builds the same thing in lockstep, and
each task depends on the one before it. Tasks 4 and 5 are the only genuinely
independent pair, and both are optional.
