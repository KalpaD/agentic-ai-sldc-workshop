# Architecture diagrams — Cash-Flow Tracker

One diagram per task in [`../tasks/todo.md`](../tasks/todo.md). Show them in
order and the system assembles itself in front of the room.

| Diagram | Task | What appears |
|---|---|---|
| [`step-1-scaffold.puml`](step-1-scaffold.puml) | Task 1 | main.ts, AppModule, ServeStaticModule, TypeORM, the empty SQLite file, a page with one heading |
| [`step-2-record-a-transaction.puml`](step-2-record-a-transaction.puml) | Task 2 | The form, `app.js`, the whole Transactions module, the entity, the `transaction` table |
| [`step-3-list-and-balance.puml`](step-3-list-and-balance.puml) | Task 3 | `GET /transactions`, `GET /summary`, the table and the balance, `style.css` |
| [`step-4-delete.puml`](step-4-delete.puml) | Task 4 *(stretch)* | `DELETE /transactions/:id`, `remove(id)`, a button per row |
| [`step-5-in-vs-out-bars.puml`](step-5-in-vs-out-bars.puml) | Task 5 *(stretch)* | Two CSS bars — browser only, the server gains nothing |

The whole system on one page, with no build order shown, is
[`../cash-flow-tracker-architecture.puml`](../cash-flow-tracker-architecture.puml).

## How to read them

Every diagram shows the **whole** system, not just the new part. Nothing ever
disappears between steps — that is the point.

- **Solid coloured border, dark text** — built in this task
- **Pale border, faint text** — already there from an earlier task

The distinction runs down to individual lines inside a box, so
`TransactionsController` in Task 4 shows `POST` and the two `GET`s in faint grey
with `DELETE /transactions/:id` in dark ink beneath them.

Colours mark the layer, and match the Day 4 slides: terracotta for the browser,
teal for NestJS, gold for the data.

The note in the corner of each diagram is that task's checklist, copied from
`todo.md`, plus the check that proves it works. A student should be able to hold
the diagram next to the todo list and see the same words.

Task 5 is worth pausing on: the entire server is faint. A new feature that costs
the backend nothing is a good thing to be able to see.

## Re-rendering

Needs PlantUML and Graphviz.

```sh
brew install plantuml graphviz
plantuml -tpng step-*.puml
plantuml -tsvg step-*.puml
```

`_theme.iuml` holds the palette, the skin and the `$add` / `$old` / `$badge`
helpers. It is included by every step and is not a diagram on its own — change a
colour there and all five follow.

Two PlantUML quirks worth knowing before editing:

- A `<color:…>` tag cannot cross a `\n`. Hence the `$add()` / `$old()` helpers —
  one call per line.
- A floating `note as x` takes `#RRGGBB` only; the `#back:…;line:…` form that
  works on components is rejected there.
