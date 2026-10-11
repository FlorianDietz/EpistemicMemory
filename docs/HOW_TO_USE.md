# How to Use This System (for humans)

This guide covers the daily workflow. The agent-facing rules live in [CLAUDE.md](../CLAUDE.md); you don't need to memorize those — your agents load them automatically — but skimming them once helps you know what behavior to expect.

## The basic loop

1. **Start a topic.** Open your agent (e.g. Claude Code) and describe what you want, or write your request at the top of a new file `convos/{you}/{date}_{topic}_LOG.md` and point the agent at it. The agent creates the LOG/SUMMARY pair and adds the topic to the README index.
2. **Work happens in files, not chat.** The agent writes its analyses, plans, and results into the LOG. Chat is for small clarifications. This feels slower on day one and pays off from day two: everything important survives the session.
3. **Comment inline.** Read the LOG (or just its latest section) and write comments directly into the file, prefixed with `C:`. You can put several comments in different places at once; the agent answers each in place, directly beneath your comment. This is the main way you steer.

   ```
   The agent wrote some analysis here.
       C: I don't think assumption 2 holds, we dropped that API last month.
   ```

4. **Next session, any agent can continue.** A fresh agent reads the SUMMARY, not your chat history. If it needs detail, it greps the LOG. You stop re-explaining the project from scratch.

## Reviewing agent work

Agents in this system do not commit or push unless you explicitly ask them to. You review their changes with `git diff`, then commit what passes. This loop is the human oversight layer:

- Everything an agent wrote is visible in one place before it becomes permanent.
- "Committed" acquires a real meaning: a human looked at this (a quick look — the system deliberately calls this *spot-checked, not verified*).
- If you use the private-records feature, the diff review is also the last filter that keeps private content from reaching the shared repository.

Review honestly but don't aim for line-by-line reading of everything; the epistemic markers exist precisely so that unverified content is labeled as such inside the files.

Useful git commands if you're not fluent:

```
git status          # what changed
git diff            # review all uncommitted changes
git add -A && git commit -m "message"   # accept the changes
git restore <file>  # throw away changes to one file
```

Using an IDE such as VS Code is highly recommended: They provide a GUI to inspect git changes more quickly.

## Epistemic status markers — what they mean for you

Agents tag claims in the files: `[decided]`, `[verified: ...]`, `[concluded]`, `[assumed]`, `[superseded]`, `OUTDATED`. Two things matter for you:

- An agent may never mark its own proposal `[decided]`. If you agree with a proposal, say so — in a `C:` comment or chat — and the agent records the ratification. If you see `[concluded]` on something you consider settled, that's your cue to ratify it.
- If you spot a claim that's wrong, don't just fix the text: tell the agent, so the correction lands in the LOG with an `OUTDATED` marker on the old claim. That way no future agent resurrects it.

## Teams

Each team member has their own folder under `convos/`. Agents work in the current user's folder, but they are also instructed to skim the rest of the team's recent SUMMARYs and flag related work ("Maria already analyzed this last month"). There is no merge machinery and no ownership bureaucracy: if two people's decisions conflict, that's a conversation between humans, and its outcome gets recorded like any other decision.

All team members write comments with the same `C:` prefix; whose comment it is follows from whose folder the file is in.

## Private records (experimental)

If some of your material is sensitive (personal topics, unfiltered notes, confidential context), keep it in `convos/{you}/private/` — git never sees that folder. Topics there use the same conventions with P-prefixed names (`..._PLOG.md`, `..._PSUMMARY.md`). Everything in a private log stays private unless you mark it shareable (a `SHARE:` prefix on a passage, a `[shareable]` tag on a section, or just telling the agent informally what may be shared — "everything except the part about my boss"). When shared content exists, the sync produces a public SUMMARY containing only that content, and you review it in the diff like everything else.

Two honest warnings: this feature is young and not yet thoroughly tested, and the private folder is not backed up by the project repository — set up your own backup (an easy option: make the private folder its own git repository with a private remote; the outer repo ignores it entirely).

## Git worktrees (optional, for parallel topics)

If you run several long-lived topics at once, git worktrees give each branch its own directory, so slow-burning work doesn't block review of everything else. The conventions handle this well: merge conflicts concentrate in the README index and are usually trivial for an agent to resolve.

One trap to know: gitignored folders (like your private folder) exist only in the worktree where you created them. Keep one canonical private folder and symlink it from other worktrees:

```
ln -s /path/to/main-worktree/convos/you/private /path/to/other-worktree/convos/you/private
```

And remember that work on an unmerged branch is invisible elsewhere — agree with yourself (or your team) on a merge cadence.

## Text for outside readers

When an agent drafts something that leaves the project (an email, a post, an application), it runs a check against a list of tells that make text read as AI-written. Many readers stop reading at the first cluster of them, and some forums reject such posts automatically. The first time this comes up, the agent will ask you for a few samples of your own writing, done without AI help, so it can draft in your voice instead of its own. Your `C:` comments count as samples too. You can decline; it then only does the checklist pass. Either way, you do the final edit yourself. Some venues require that, and readers can tell when nobody did.

## Maintenance

Two habits keep the system healthy; both are one sentence to trigger:

- **Sync** (`!sync` or "please sync the logs"): an agent checks that SUMMARYs reflect their LOGs, cross-references are two-way, and nothing agreed in a LOG got silently dropped. Run it when a lot has changed, or roughly monthly.
- **Feedback review**: occasionally ask an agent to go through FEEDBACK.md and propose which recurring problems deserve a new rule. This is the mechanism that keeps the rules improving over time.
