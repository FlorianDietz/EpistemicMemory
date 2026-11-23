# Code Review Procedure

When you perform a code review, check coding projects for quality, consistency, and compliance with the project's guidelines.

**What the reviewing sub-agent checks:**
1. **Convention compliance**: does the project structure match `docs/CODING.md`? Are the critical rules followed (Fail Loudly, Observe & Verify)?
2. **Documentation consistency**: does the SUMMARY match the actual implementation? Are data-format examples accurate?
3. **Code quality**: assertions, error handling, no silent failures, no bare `except:`?
4. **Data integrity**: do referenced data files exist? Are they in the expected format?
5. **Build vs. decisions**: before a build counts as done, list the topic's `[decided]` items and planned steps, including those decided after the plan was written, and check each against the code: what it does, and where in the flow it runs (before or after which step, in which process). A step that runs in a different place than decided is a deviation to report to the user, even when it does the same thing. Report it; don't move it back on your own.
6. **Real-data readiness**: check the LOG for a recorded run of every build script, data check and runner on the full real inputs in the real environment. Without one, the build is not ready; tests passing on synthetic fixtures don't establish readiness.

**Modes:**
- **Report-only**: the sub-agent writes a report to `convos/{user}/{date}_code_review_LOG.md`. No files modified. Use when another agent is active or when results have already been used.
- **Fix mode**: the sub-agent writes the report AND directly fixes issues. Use for maintenance on idle projects.

## Error-injection check (seeded defects)

A way to *measure* whether a review was actually thorough, instead of trusting a "looks good ✓". You plant deliberate errors and see if the reviewer finds them all; if it misses planted errors, assume subtler real ones were missed too (the catch rate is a one-sided alarm — good for failing a review, not for certifying one). It works for prose review (LOG/SUMMARY/draft accuracy) as much as for code. Use it whenever the work contains **derived or conclusion content** — values or claims you could only check by re-deriving prior steps; a pure-plumbing change has nothing worth planting.

Procedure:
1. After writing, pick the high-value spots — conclusions, end results of background calculations, numbers verifiable only by re-derivation.
2. **Save the undo before you plant.** Copy every file you will plant in to `scratch/plant_backup/` (`mkdir -p scratch/plant_backup && cp <file> scratch/plant_backup/`), so you can revert no matter what happens next. Don't use a `git diff` patch for this: in this workflow the working tree normally holds uncommitted LOGs, SUMMARYs and the user's own edits, and reversing a whole-tree patch would delete them along with the plants. (`scratch/` is gitignored, so the backups stay invisible to a reviewer checking `git status`. Don't rely on memory: an agent planter can lose its context before it reverts.) Ask the user not to edit those files until the revert, since restoring the backups would overwrite their edits.
3. Plant a few deliberate errors there, mixing obvious and subtle ones.
4. Spawn the reviewer **clean** — a fresh sub-agent given *only* the material to review, **not** pointed at this file or the repo's git history. That prevents it from realizing it's being tested and from finding the plants via `git diff` or file timestamps. Prompt it honestly but without a count: *"there are a few deliberate errors in here — find them all."* A specific number invites it to stop after finding that many.
5. After the review, revert by copying each backup back over its file, then confirm with `diff` that every file matches its backup exactly. A planted error left in the project is exactly the silent corruption this system exists to prevent. While plants are live, nobody commits, agents included: a commit made then can put a deliberate error into the history.
6. Backstop: if your context is about to be compacted before you have reverted, record the planted errors and the backup folder in whatever survives (the topic LOG), so the next agent finishes the revert. The plants must not be forgotten.
