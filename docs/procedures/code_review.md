# Code Review Procedure

When you perform a code review, check coding projects for quality, consistency, and compliance with the project's guidelines.

**What the reviewing sub-agent checks:**
1. **Convention compliance**: does the project structure match `docs/CODING.md`? Are the critical rules followed (Fail Loudly, Observe & Verify)?
2. **Documentation consistency**: does the SUMMARY match the actual implementation? Are data-format examples accurate?
3. **Code quality**: assertions, error handling, no silent failures, no bare `except:`?
4. **Data integrity**: do referenced data files exist? Are they in the expected format?

**Modes:**
- **Report-only**: the sub-agent writes a report to `convos/{user}/{date}_code_review_LOG.md`. No files modified. Use when another agent is active or when results have already been used.
- **Fix mode**: the sub-agent writes the report AND directly fixes issues. Use for maintenance on idle projects.

## Error-injection check (seeded defects)

A way to *measure* whether a review was actually thorough, instead of trusting a "looks good ✓". You plant deliberate errors and see if the reviewer finds them all; if it misses planted errors, assume subtler real ones were missed too (the catch rate is a one-sided alarm — good for failing a review, not for certifying one). It works for prose review (LOG/SUMMARY/draft accuracy) as much as for code. Use it whenever the work contains **derived or conclusion content** — values or claims you could only check by re-deriving prior steps; a pure-plumbing change has nothing worth planting.

Procedure:
1. After writing, pick the high-value spots — conclusions, end results of background calculations, numbers verifiable only by re-derivation.
2. Plant a few deliberate errors there, mixing obvious and subtle ones.
3. **Save the undo before you review.** Capture the plant as a patch immediately — `mkdir -p scratch && git diff > scratch/plant.patch` — so the revert is bulletproof no matter what happens next. (`scratch/` is gitignored, so the patch stays invisible to a reviewer checking `git status`. Don't rely on memory: an agent planter can lose its context before it reverts.)
4. Spawn the reviewer **clean** — a fresh sub-agent given *only* the material to review, **not** pointed at this file or the repo's git history. That prevents it from realizing it's being tested and from finding the plants via `git diff` or file timestamps. Prompt it honestly but without a count: *"there are a few deliberate errors in here — find them all."* A specific number invites it to stop after finding that many.
5. After the review, revert: `git apply -R scratch/plant.patch`, then confirm `git diff` shows **zero** residual change. A planted error left in the project is exactly the silent corruption this system exists to prevent.
6. Backstop: if your context is about to be compacted before you have reverted, record the planted errors and the patch path in whatever survives (the topic LOG), so the next agent finishes the revert. The plants must not be forgotten.
