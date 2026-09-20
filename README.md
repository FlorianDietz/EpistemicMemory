# Epistemic Memory

File conventions that give agent-assisted projects a persistent, human-auditable memory.

LLM agents forget everything between sessions. The usual fixes (chat history, vector stores, auto-generated summaries) either drown the next session in noise or quietly lose the reasoning behind decisions. This template takes a different route: **all project memory lives in plain markdown files, under two complementary conventions**:

- Every discussion topic gets a **LOG** file: an append-only chronological record. It may contain dead ends and corrections, and later parts may contradict earlier parts, because decisions change. Nothing is ever rewritten, so reading top-to-bottom always gives the true sequence of events.
- Each LOG is paired with a **SUMMARY**: a short, contradiction-free statement of the current state ("we initially chose X but switched to Y, because Z"). A fresh agent reads the SUMMARY and can continue the work.

The LOG is optimized for trust, the SUMMARY for fast orientation. With only one of the two, you either lose the reasoning behind decisions or force every new agent to read the whole history.

Around that core sit a few conventions that earn the "epistemic" in the name:

- **Epistemic status markers** — claims in the files are tagged `[decided]`, `[verified]`, `[concluded]`, `[assumed]`, `[superseded]`, or `OUTDATED`, so a fresh agent can't mistake a working assumption for a settled decision. Agent proposals stay `[concluded]` until a human ratifies them.
- **Inline human comments** — you write comments directly into the files with a `C:` prefix; agents answer in place. Only humans may use the prefix, so authorship of every line is checkable.
- **No numbers from memory** — agents must re-read the source file before writing any specific number, because a figure carried across a session boundary is often subtly wrong.
- **Human review via git** — agents don't commit on their own; you review their changes in the diff before committing. That review loop is the human oversight layer of the whole system.
- **Team support** — each user has their own conversation folder; agents cross-read the team's summaries and flag related work. A solo project is just a team of one.
- **Private records** (experimental) — a gitignored per-user folder with the same conventions, for content that must never reach the shared repository, with explicit rules for deriving shareable summaries from private logs.

I built this for my own AI-alignment research project and have used it daily since January 2026; the research behind it is partly funded by the Foresight Institute, and a simplified version was used by the four-person mentorship team (three mentees and me) of a related research project. It is written for [Claude Code](https://claude.com/claude-code); the conventions themselves have little that is Claude-specific, and porting guides for other agent harnesses are on the roadmap (see [FUTURE_WORK.md](FUTURE_WORK.md)).

## Quickstart (humans)

1. Click **"Use this template"** on GitHub (or clone this repo) to create your own project repository.
2. Open the repository in Claude Code.
3. Say: **"Set this project up for me."** The agent will follow the checklist below, interview you, and personalize everything.
4. Read [docs/HOW_TO_USE.md](docs/HOW_TO_USE.md) — ten minutes that explain how to work with the system day to day.

A worked example of the conventions is in `convos/jamie/` (a fictional user with a fictional topic; setup removes it).

## Setup checklist (agents)

If you are an agent reading this and the repository still contains the demonstration user `convos/jamie/`, this is an unconfigured template. Your task is to adjust it to the user. Work through this checklist:

1. **Interview the user.** You need: (a) the name of each team member (one name is fine); (b) a short description of what the project is about; (c) whether they want the private-records feature set up now (see CLAUDE.md §Privacy); (d) their answer on the git workflow, see step 5.
2. **Create the user folders**: `convos/{user}/` per team member, using short lowercase slugs. For teams, record in the README how agents identify the current user (recommended: match `git config user.name` against the team list; ask when ambiguous).
3. **Personalize the README**: replace this file's entire content with the working version in [docs/README_SKELETON.md](docs/README_SKELETON.md), filled in with the project description and team list. Keep the skeleton's link back to this template's repository (that is how future readers find the origin and how updates and the upcoming memory-harness release are announced).
4. **Remove the demonstration content**: delete `convos/jamie/` and the template note in CLAUDE.md. In FUTURE_WORK.md, follow the setup instruction in the file itself (delete the template paragraph and example entries; an empty list is fine). In FEEDBACK.md, keep the header and delete any entries below the ruler (they belong to the template repository, not to this project).
5. **Propose the git workflow.** The default rule is that agents never commit or push; the human reviews every change in the diff before committing it. Present this as the recommended default and explain what it buys: the diff review is how agent-written records get human oversight, it is what makes "committed" mean "a human looked at this", and it is the final filter that keeps private content out of the shared repository. If the user declines and wants agents to commit, adjust these places: CLAUDE.md §"API keys and git" (the rule itself and the "Committed = spot-checked" note), CLAUDE.md §Privacy point 4 (the diff-review sentence), docs/CODING.md §"User Interaction for Coding Projects" (the git parenthetical), docs/HOW_TO_USE.md §"Reviewing agent work", and docs/HOW_TO_USE.md §"Private records" (the review-in-the-diff sentence).
6. **Set up private folders** if requested: create `convos/{user}/private/` with its `README.md` index (the `.gitignore` already excludes it). If the user works with git worktrees, add the symlink described in CLAUDE.md §Git worktrees. Sanity check while you're here: only the current user's folder may contain `private/`; if one exists under another user's name, something has gone wrong — flag it.
7. **Confirm the result**: show the user the new structure, and record the setup itself as the project's first LOG/SUMMARY pair (the team list, the git-workflow choice, and any other decisions from the interview — this is the record future agents check these against). Then start the user's first real topic.

## What's in the box

| File | Purpose |
|------|---------|
| [CLAUDE.md](CLAUDE.md) | The core conventions, loaded by the agent every session |
| [docs/HOW_TO_USE.md](docs/HOW_TO_USE.md) | Human-facing guide: daily workflow, git basics, review, teams, worktrees |
| [docs/CODING.md](docs/CODING.md) | Coding conventions for projects that include code |
| [docs/procedures/](docs/procedures/) | On-demand procedures: sync, code review, multi-agent debate, self-containedness check |
| [docs/README_SKELETON.md](docs/README_SKELETON.md) | The working README your project gets after setup |
| [FUTURE_WORK.md](FUTURE_WORK.md) | Example TODO file — doubles as this template's real roadmap |
| [FEEDBACK.md](FEEDBACK.md) | Where agents record workflow problems and how to prevent them |
| convos/jamie/ | Fictional demonstration topic showing the conventions in action |

## Roadmap

This template is the conventions layer of a larger system. The same private project that produced it also runs a **memory harness** on top of these files: an API proxy plus tool server that gives agents a stochastic working memory over a shared store — memories that get injected, reinforced, deprecated, and audited. It is in daily dogfooding use; the plan is to release it into this repository once it is untangled and hardened for other people's machines. [FUTURE_WORK.md](FUTURE_WORK.md) has the description; watch this repository if you want the release.

## License

MIT — see [LICENSE](LICENSE).
