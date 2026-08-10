# Cash-Flow Tracker — Sunrise Bakery

A small tool for a bakery owner to record money coming in and going out, and see
where they stand at the end of a day.

## Stack

- Backend: NestJS
- Database: SQLite via TypeORM, `synchronize: true`
- Frontend: plain HTML, CSS and JavaScript in `/public` — no framework

## Money

- Amounts are whole rupees, no decimals
- Always store an amount as a number, never as text
- Money in and money out are both positive; the in/out choice decides direction

## Rules

- No login, no user accounts
- One `Transaction` entity only; categories are a fixed list
- Do not use git and do not commit — we are not using version control today
- Do not write new test files; verify by running the app and using it
- Keep every file inside this project folder
- Ask before adding any new dependency
