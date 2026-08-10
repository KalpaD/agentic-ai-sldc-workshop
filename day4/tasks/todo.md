# Todo: Cash-Flow Tracker — Sunrise Bakery

Work top to bottom. **Finish one task, check it, then start the next** — never
two at once. Full detail for each task is in `tasks/plan.md`.

---

## Phase 1: Foundation

- [ ] **Task 1 — Scaffold the project and serve a page**
  - [ ] NestJS project created
  - [ ] TypeORM connected to `cashflow.sqlite`
  - [ ] Static files served from `/public`
  - [ ] `npm run start:dev` runs with no errors
  - [ ] `localhost:3000` shows a page saying "Cash-Flow Tracker"

### ✋ Checkpoint: Foundation
- [ ] Dev server running
- [ ] Page loads in the browser
- [ ] `cashflow.sqlite` exists
- [ ] **Whole room passes before anyone moves on**

---

## Phase 2: Core Features

- [ ] **Task 2 — Record a transaction**
  - [ ] `Transaction` entity: id, date, description, category, type, amount
  - [ ] `amount` is stored as a number, not text
  - [ ] `POST /transactions` saves and returns the record
  - [ ] Form on the page: date, description, category dropdown, in/out toggle, amount
  - [ ] Submitting clears the form and confirms the save
  - [ ] Record survives a server restart

- [ ] **Task 3 — See the money and the balance**
  - [ ] `GET /transactions` returns all records, newest first
  - [ ] `GET /summary` returns totalIn, totalOut, balance
  - [ ] Table of transactions on the page
  - [ ] Balance visible at the top, updates after each entry

### ✋ Checkpoint: Core Features
- [ ] Enter the three test transactions:
  - [ ] Morning bread sales · Sales · **in** · 5000
  - [ ] Flour and sugar · Supplies · **out** · 1200
  - [ ] Electricity · Utilities · **out** · 800
- [ ] **Balance reads 3000**
- [ ] Data survives a restart
- [ ] This is a complete, useful tool — everything below is optional

---

## Phase 3: Polish *(stretch — skip without apology if time is short)*

- [ ] **Task 4 — Delete a transaction**
  - [ ] `DELETE /transactions/:id`
  - [ ] Delete button on each row
  - [ ] Balance updates immediately

- [ ] **Task 5 — Show in-vs-out as bars**
  - [ ] Two proportional bars, built from `div` elements only
  - [ ] Each labelled with its amount
  - [ ] No charting library

### ✋ Checkpoint: Complete
- [ ] Tasks 1-3 fully met
- [ ] Three-transaction test gives 3000
- [ ] Nothing from the Not Doing list has crept in
- [ ] Runs from a cold start

---

## Standing rules for every task

- **Do not use git or commit.** Not part of this session.
- **Do not write test files.** Verify by running the app and using it.
- **Something broken?** Tell the agent what you see and ask it to fix it — never
  hand-edit the code.
- **Never start two tasks at once.** Finish, check, then move.
