# FEEDBACK

A list of problems with the documentation or workflow, noted by agents (or humans), each with constructive criticism on how it could have been prevented. This is how the conventions improve instead of rotting: when an agent makes a mistake that better instructions would have prevented, the mistake gets an entry here, and a periodic review turns recurring entries into new rules.

Entry format: date, what went wrong (or what friction occurred), and — where possible — a concrete suggestion: which file should say what, so the next agent doesn't hit the same problem.

A configured project starts this list empty. Entries below the ruler, if any, are worked examples from the project this template was extracted from; they are removed during setup (README setup checklist, step 4).

---

- 26-09 (example entry, genericized from a real incident in the upstream project) — An agent continuing work after a context compaction wrote a specific result figure from memory and attached the source file as a citation; the number was wrong, and the citation made it look checked. It was caught only because the human reviewer re-derived the value. Prevention: this is what the "no numbers from memory" rule in CLAUDE.md exists for — the citation is only trustworthy if the file was re-read in the same session. Suggested addition at the time (since adopted): after any compaction, re-read the active LOG/SUMMARY instead of trusting the continuation summary's description of them.
