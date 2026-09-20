# Synchronization Procedure

LOG and SUMMARY files drift out of sync (an agent gets interrupted, or forgets to update the SUMMARY after LOG changes). This procedure fixes that.

1. **When to sync**: when the user asks (they write `!sync` or ask you to sync the logs), or when you suspect drift (SUMMARY dates older than LOG activity; a SUMMARY listing "unanswered" C: comments that have since been answered).
2. **How to sync**: launch one or more sub-agents, each tasked with:
   - Reading a LOG file and its corresponding SUMMARY
   - Checking that all key decisions, conclusions, and open questions in the LOG are reflected in the SUMMARY
   - Checking that all C: comments are accounted for (answered in the LOG → noted in the SUMMARY; unanswered → listed in the SUMMARY)
   - **Making cross-references two-way.** When this topic references another topic, ensure the other topic's SUMMARY carries a reverse "See also" backlink — and add the missing direction (annotated with *why* the topics relate), don't just verify it. An agent reading either topic should discover the other.
   - **Checking for dropped promises.** Grep the LOG for agreed bookkeeping side-effects ("add X to FUTURE_WORK", "make sure we don't forget") and verify each was executed — these get silently dropped when a session's main deliverable ships.
   - Checking formatting compliance (epistemic markers, entry headers, dates)
   - Updating the SUMMARY if needed, with the sync date in its `Last updated` line
3. **Private topics**: a PLOG syncs to its PSUMMARY, and — only if it contains shared-marked content — to a public SUMMARY per CLAUDE.md §Privacy. Sync agents operate only on the **current user's** files; syncing another user's private folder is never correct (on this machine it shouldn't even exist — flag it if it does).
4. **Scope**: a single pair, one user's folder, or the whole project.
5. **Afterwards**: note the sync in the README (e.g. a maintenance row's "last run" date, if the project keeps one).
