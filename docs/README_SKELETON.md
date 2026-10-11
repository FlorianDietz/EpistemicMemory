# {Project Name}

> This is the working-README skeleton. During setup, the agent fills it in and replaces the repository's `README.md` with it. Curly-brace placeholders get replaced; the comments in (parentheses) get deleted.

{One or two paragraphs: what this project is about, who works on it, what a new reader should know first.}

This project uses the [Epistemic Memory](https://github.com/FlorianDietz/EpistemicMemory) conventions: all project memory lives in LOG/SUMMARY file pairs under `convos/`, governed by [CLAUDE.md](CLAUDE.md). Human guide: [docs/HOW_TO_USE.md](docs/HOW_TO_USE.md). Open ideas and deferred items: [FUTURE_WORK.md](FUTURE_WORK.md). Workflow problems and lessons: [FEEDBACK.md](FEEDBACK.md). (Keep this paragraph — it tells future readers where everything lives and where the conventions come from, and the template repository is where updates get announced.)

## Team

(One bullet per member: name, folder slug, anything agents should know. For a team of one, say so — then no user-identification step is needed.)

- {Name} — `convos/{slug}/`
- {Name} — `convos/{slug}/`

(For teams: state how agents identify the current user. Recommended: match `git config user.name` against this list; if the match is ambiguous, the agent asks before proceeding.)

## Current status and goals

{What is currently being worked on, top priorities, anything time-sensitive. Keep this section short and current — replace outdated content instead of appending to it; history lives in the topic LOGs.}

## Maintenance

(Optional section. Recurring tasks and one-time deadlines that agents should check at session start. Delete it if unused.)

Recurring tasks:

| Task | Last run | Due when | Notes |
|------|----------|----------|-------|
| Sync (LOG/SUMMARY drift) | — | After many LOG changes, or ~monthly | `!sync`; procedure: `docs/procedures/sync.md` |
| LLM-ism checklist refresh (delete this row if the project publishes nothing) | — | ~Monthly while you publish regularly; otherwise before any high-stakes submission | Update the checklist and Known-screening sections of `docs/procedures/outward_text.md` from full primary sources (see the file's refresh rule); bump its refresh date |

One-time deadlines (delete a row when done):

| Due | Item | Source (authoritative) |
|-----|------|------------------------|
| {date} | {what has to happen by then} | {which LOG/SUMMARY holds the details} |

## Conversation Index

All discussions live in `convos/{user}/`. **When starting a new topic, add a row here.** Read the SUMMARY first; open the LOG only if the SUMMARY is insufficient. Status cells describe current state only — rewrite them on update, don't append.

| Date | User | Topic | Status | SUMMARY | LOG |
|------|------|-------|--------|---------|-----|
| {date} | {user} | {topic} | {status} | [SUMMARY](convos/{user}/{file}) | [LOG](convos/{user}/{file}) |
