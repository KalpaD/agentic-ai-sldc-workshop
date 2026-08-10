# Day 4 Capstone — Cash-Flow Tracker

## Problem Statement

How might we give ICT teachers a genuinely real, non-toy full-stack build to
complete in one 2-hour session, so they leave having directed an agent to ship
working software — not a classroom demo?

## Recommended Direction

A single-user **Cash-Flow Tracker** for Sunrise Bakery — the same business whose
landing page they built on Day 3. Log money in and out, see a running balance.

- **Backend:** NestJS
- **Database:** SQLite via TypeORM
- **Frontend:** plain HTML, CSS and JavaScript — no framework, no build step
- **Runs locally.** No deployment, no accounts, no LLM calls in the app itself

Day 3 built what the bakery's customers see. Day 4 builds what the owner uses
behind the counter. Same fictional business, two sides of one story.

---

## The three skills this session runs on

These live in `day4/` and are the advanced versions — the same skills a working
software team would use. Teachers copy all three into `.opencode/skills/`.

| Skill | Stage | What it produces |
|---|---|---|
| `idea-refine` | 1 | A one-pager: problem, direction, MVP scope, **Not Doing** list — *demonstrated by the facilitator; the room takes a shared copy* |
| `planning-and-task-breakdown` | 2 | A plan sliced **vertically**, with checkpoints |
| `incremental-implementation` | 3 | The build discipline: implement → test → verify → next slice |

**What changed from the earlier plan.** Day 2 taught a three-skill chain ending
at a task list. Day 4 adds `incremental-implementation`, which governs *how the
code actually gets written*. That single addition is what restructures this
session — see "Vertical slices" below.

> `write-prd` from Day 2 is **not** used here. `idea-refine` already produces a
> one-pager with an MVP Scope and a Not Doing list, which is all this build
> needs. Adding a second document stage would cost 10 minutes and produce
> nothing new.

---

## Vertical slices — the biggest change to this session

Both advanced skills insist on slicing **vertically**, not by technical layer.
The original plan was horizontal, and it had a real flaw:

| Horizontal (old) | Vertical (now) |
|---|---|
| Task 1: scaffold | Slice 0: scaffold — prove it runs |
| Task 2: entity + database | Slice 1: **add a transaction** — entity + POST + a form |
| Task 3: the REST API | Slice 2: **see your transactions** — GET + list + balance |
| Task 4: the frontend | Slice 3 *(stretch)*: delete + summary bars |
| ⚠ Nothing visible until ~95 min | ✅ Something works end to end by ~55 min |

With the old order, a teacher who ran out of time had a database and no
application. With vertical slices, whoever stops early still has **a working
thing that does something** — which matters enormously for a room that has
never shipped software before.

Each slice ends in a state you can open in a browser and use.

---

## The project AGENTS.md

Write this in Slice 0, before any code. It exists to adapt three professional
skills to a two-hour teaching session — and demonstrating that is itself worth
saying out loud to the room.

```markdown
# Cash-Flow Tracker — Sunrise Bakery

## Stack
- Backend: NestJS
- Database: SQLite via TypeORM, synchronize: true
- Frontend: plain HTML, CSS and JavaScript in /public — no framework

## Money
- Amounts are whole rupees, no decimals
- Store amount as an integer column, never as text
- Convert the amount with Number() before sending it from the page
- Reject any amount that is not a positive whole number

## Rules
- No login, no user accounts
- One Transaction entity only; categories are a fixed list
- Do not use git and do not commit — we are not using version control today
- Do not write new test files; verify by running the app and using it
- Keep every file inside this project folder
- Ask before adding any new dependency
```

**Why those two unusual lines.** The `incremental-implementation` skill ends
every slice with *commit your work* and *run the test suite*. Both are correct
for a professional team and wrong for this room — nobody here has used git, and
writing tests would consume the session. Rather than editing the skill, we
override it in `AGENTS.md`.

That is a genuinely useful lesson: **you adapt a professional skill to your
situation with project rules, not by rewriting the skill.**

---

## MVP Scope

**In:**
- `Transaction` entity — date, description, category, type (in/out), amount
- Endpoints: create, list, delete, and a summary returning total in, total out,
  and balance
- One page: a form to add a transaction, a table of transactions, and a running
  balance

**Out — the Not Doing list:**

| Not doing | Why |
|---|---|
| Login / accounts | Sessions and auth is a whole separate topic |
| Editing transactions | Add + delete is enough CRUD surface; editing is a stretch |
| Category management | Hardcoded list; a second entity doubles the complexity |
| Charting library | Plain CSS bars keep the frontend at zero dependencies |
| Deployment | Runs locally and is demoed live |
| Git / version control | Not taught in this course; excluded in `AGENTS.md` |

---

## Facilitator Runbook — 2 hours, lockstep

**Standing rule, stated once up front:** if something on screen doesn't match,
nobody debugs by hand — they tell their agent what they see and ask it to fix
it. That is the skill being tested today, not NestJS.

### Stage 0 — Ready check · 5 min · ends 0:05

Everyone opens a terminal and runs `opencode`. Confirm the status line mentions
**Google**.

**Checkpoint:** hands up if the status line doesn't say Google. Fix with
`/connect` before anything else — a broken key here stops the whole session.

### Kickoff & brief · 8 min · ends 0:13

**Say:** "Yesterday you built the bakery's shop window. Today you build the till
— the tool the owner actually uses. Same three-step method as Tuesday, plus one
new skill that governs how the code gets written."

Show the three skills and say plainly: **these are the real thing.** Not
simplified teaching versions — the same skills a working software team uses.

**Do now — everyone installs the three skills:**

```
> copy the three skill folders from the day4 folder into
  .opencode/skills/ — idea-refine, planning-and-task-breakdown
  and incremental-implementation. Then list my skills.
```

**Checkpoint:** everyone lists three skills before moving on.

### Stage 1 — Refine the idea · 15 min · ends 0:28

**You demonstrate; the room watches. Then everyone takes the same PRD.**

Running `idea-refine` live on 79 laptops would produce 79 different PRDs, and
every later stage would drift apart. So this stage is a demo followed by a
shared handout — everyone starts Stage 2 from an identical document.

#### 1. Demonstrate the process — 7 min, on the projector only

Run it for real. Don't narrate a slide.

```
> use the idea-refine skill on this: a small tool for Sunrise Bakery
  to track money coming in and going out, so the owner can see where
  they stand at the end of a day
```

Answer its questions out loud as they come, so the room hears the reasoning:

| When it asks about | Answer with |
|---|---|
| Who it's for | The bakery owner, on a laptop behind the counter |
| What success looks like | Seeing today's balance without a calculator |
| How long it should take to use | Under ten seconds to record a sale |
| What to leave out | Login, editing, reports, anything with a chart library |
| Which direction | The plainest one — one page, one list, one balance |

**Three things to point at while it runs:**

- **It refuses to start writing.** It asks first. That's the skill's method, and
  it's the opposite of what people expect from AI.
- **It offers options and recommends one.** Say why you're picking the plainest
  — richer versions are better products but can't be understood in a session.
- **The Not Doing list.** Land this hard: it's the section that keeps a build
  finishable. Everything on it was a reasonable idea that got cut on purpose.

> Keep this to seven minutes. If the skill goes long, cut it off and move on —
> the room has seen the shape, and the finished document is coming next.

#### 2. Hand out the finished PRD — 5 min

**Say:** "Mine took a few minutes and yours would be slightly different. So that
we all build the same thing, here's the finished one."

Share `day4/cash-flow-tracker-prd.md`. Everyone saves it into their project:

```
> create docs/ideas/cash-flow-tracker.md with this content:
  [paste the PRD]
```

#### 3. Read it together — 3 min

Read the **MVP Scope** and **Not Doing** sections aloud. These two decide
everything that happens for the rest of the session.

**Checkpoint:** everyone has `docs/ideas/cash-flow-tracker.md` saved, and can
point to the line that says no login. Same document, same starting line.

> **Why this is the right call, if anyone asks:** a real team agrees one spec
> and works from it. Twelve people each with their own private PRD is not how
> software gets built — it's how projects fail.

### Stage 2 — Plan and slice · 12 min · ends 0:40

```
> use the planning-and-task-breakdown skill on
  docs/ideas/cash-flow-tracker.md. Slice vertically — each slice
  must end with something I can open in a browser and use.
```

The instruction to slice vertically matters. Left alone the skill may still
produce layers; saying it explicitly gets the shape we want.

**Reference plan** — what a good breakdown looks like. The worked version is in
`tasks/plan.md` and `tasks/todo.md`, ready to hand out if a room's plan drifts:

| Task | Phase | Ends with |
|---|---|---|
| 1 — Scaffold | Foundation | `npm run start:dev` runs, page at localhost:3000 |
| 2 — Record a transaction | Core | A form that saves a record that survives restart |
| 3 — See the money | Core | A table plus a running balance |
| 4 — Delete *(stretch)* | Polish | Remove a row, balance updates |
| 5 — Summary bars *(stretch)* | Polish | Proportional CSS bars, no library |

**Checkpoint:** hands up if a plan has more than five tasks, or if Task 2
doesn't produce something usable on its own. Trim before building — or hand out
`tasks/todo.md` and move on if time is tight.

### Stage 3 — Build, one slice at a time · 48 min · ends 1:28

**Say:** "From here the incremental-implementation skill drives. It has one rule
we care about: finish a slice, check it works, then start the next. Never two at
once."

```
> use the incremental-implementation skill to build Task 1
  from tasks/todo.md
```

Then, before Slice 1, write the project rules:

```
> create an AGENTS.md with these rules: [read the block above]
```

**Slice 0 — Scaffold · ~14 min.** NestJS project, TypeORM + SQLite connected,
static files served from `/public`.
*Verify:* `npm run start:dev` runs, browser shows something at localhost:3000.
**Checkpoint:** everyone's dev server is running. Do not proceed past this — all
later slices depend on it.

**Slice 1 — Add a transaction · ~20 min.** The `Transaction` entity, a POST
endpoint, and a form on the page. This is the slice that makes it an app.
*Verify:* type a sale into the form, submit, and get a confirmation.
**Checkpoint:** everyone has saved one real transaction. This is the moment it
becomes software — let the room enjoy it.

**Slice 2 — See your money · ~14 min.** A GET endpoint, a table, and the running
balance.
*Verify:* the transaction from Slice 1 appears in the table with a correct
balance.
**Checkpoint:** everyone can see their data.

**Slice 3 — Tidy up · stretch only.** Delete buttons and CSS summary bars. For
pairs who are ahead; skip without apology if the room is behind.

### Stage 4 — Test it together · 10 min · ends 1:38

Everyone enters the same three transactions, called out one at a time:

| Description | Category | Type | Amount |
|---|---|---|---|
| Morning bread sales | Sales | in | 5000 |
| Flour and sugar | Supplies | out | 1200 |
| Electricity | Utilities | out | 800 |

**Everyone should see a balance of 3000.** A different number is a bug to find
*with the agent's help* — that's the intended final rep, not a failure.

### Stage 5 — Iterate, then close · 14 min · ends 1:52

Pick one new requirement live and run it through in miniature:

```
> add a filter so I can see only money going out
```

**Then close on the arc of the week:**

| Day | What they did |
|---|---|
| 1 | Understood what an agent is |
| 2 | Set up, and taught it their own commands and skills |
| 3 | Controlled its context, and directed three agents at once |
| 4 | Shipped working software with a database |

Final point to land: **they used the real skills.** Not teaching versions — the
same ones a professional team uses. What they take home works on Monday.

---

## Timing summary

| Stage | Length | Ends |
|---|---|---|
| Ready check | 5 min | 0:05 |
| Kickoff, install skills | 8 min | 0:13 |
| 1 — idea-refine | 15 min | 0:28 |
| 2 — plan and slice | 12 min | 0:40 |
| 3 — build slices 0–2 | 48 min | 1:28 |
| 4 — test together | 10 min | 1:38 |
| 5 — iterate and close | 14 min | 1:52 |

**8 minutes of slack**, plus Slice 3 as a stretch to absorb a fast room.

---

## What to expect from the advanced skills

These are written for professional teams, so they will occasionally reach for
habits this room doesn't have. All three are handled by `AGENTS.md` — but know
what's coming:

| It may try to | Why | What to say |
|---|---|---|
| Commit after each slice | `incremental-implementation` ends each cycle with a commit | Covered by the no-git rule. If it asks anyway: "no git today." |
| Run or write tests | The skill's verify step defaults to a test suite | We verify by using the app. `npm test` on a scaffold is harmless if someone tries it. |
| Add a feature flag | Rule 3, for incomplete features | Not needed — every slice we ship is complete. |
| Enter "plan mode" | `planning-and-task-breakdown` Step 1 | Fine, and useful. Let it. |
| Produce more than 5 slices | It plans for real projects, not 90-minute ones | Trim at the Stage 2 checkpoint. |

## Assumptions to validate before the day

- [ ] Dry-run the whole build once, on a free Gemini key, watching for rate
      limits during Slice 1 — the heaviest stage
- [ ] Confirm the scaffold genuinely completes in ~14 minutes on a slow laptop
- [ ] Confirm all three skills load and are listed after being copied in
- [ ] Dry-run the Stage 1 demo once and time it — seven minutes is tight if the
      skill asks a lot of questions
