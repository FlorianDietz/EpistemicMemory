# Self-Contained Check (artifact sweep)

For any artifact that will be read without the current context — code, presentations, emails, reports, shared documents. Writers deep in a task systematically under-explain because the context feels obvious; LLMs especially will not fix this unless explicitly prompted. This sweep catches it. (The underlying rule is "Self-Contained Artifacts" in CLAUDE.md.)

1. Have a reader with **no context** — a fresh sub-agent given ONLY the artifact — read it and list: references it cannot resolve, results or decisions stated but not explained, terms that seem to carry hidden meaning.
2. Then give it the LOG and other context files. Every item on its list that the context resolves is a self-containedness failure: the artifact relied on knowledge the reader didn't have.
3. Fix the artifact in place (usually easy: state the decision and its reason where it's used). Do NOT fix it by adding more references to context files — that converts the failure into a scavenger hunt.
4. Sweep for history references: statements like "this replaces X", "unlike the old rule", "as of the <date> revision" are failures even when the reader could resolve them — the artifact must read as if written in one go; its revision history lives in the LOG. (Grep candidates: "replace", "previously", "no longer", "old", "revision".) Exception, internal code only: a deliberate dead-end record per `docs/CODING.md` §Code Comments ("deliberately no caching — tried, reverted because X") is content, not a history reference — it documents the code's why. Incidental revision narration ("now handles", "changed to") is still a failure everywhere.

Calibrate the effort: for important or outward-facing artifacts, use the sub-agent (it is the honest test — you cannot un-know your own context). For small internal changes, steps 1–2 as a self-check while re-reading the artifact are enough.

When: part of finishing any coding task, presentation, email, or report that others (human or agent) will read without today's context.
