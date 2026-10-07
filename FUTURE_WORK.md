# FUTURE_WORK

This file holds open ideas and deferred work items that don't belong to any single topic; agents add entries here instead of leaving TODOs scattered in code or chat. Entries carry enough context that a future agent can pick them up cold.

**Template paragraph — during setup, delete this paragraph and all entries below the ruler; an empty list is fine.** The example entries below are real: they are this template's actual roadmap. For progress on them, watch the template repository (linked in your README).

---

- **Memory harness (the layer this template was built to feed).** The project this template comes from runs a code layer on top of these file conventions: an API proxy plus tool server that gives agents a stochastic working memory over a shared store — memories are injected into context in rotating slots, and agents can store, recall, reinforce, and deprecate them, with the file conventions as the durable ground truth underneath ("files hold data, the store holds rated pointers and hints"). It runs in daily dogfooding use, sandboxed in Docker. Releasing it here requires untangling it from private files and hardening it for machines that aren't its author's, which is why the conventions layer shipped first. Planned as a versioned release into this same repository.

- **Privacy convention: test and expand.** The private-records convention (CLAUDE.md §Privacy) is designed but not yet thoroughly tested in practice. Open points: does the SHARE-marking workflow hold up over months of real use; tooling for auditing that no private content leaked (the grep audit is a start); better backup guidance. Treat the current convention as v0.

- **Multi-user record merging.** Team support currently means per-user folders plus cross-reading. Merging several users' records into one shared hierarchy (one topic worked by three people without copy-paste between folders) is an open design problem.

- **Harness portability.** The conventions contain nothing Claude-specific except the instruction-file name (`CLAUDE.md`), but the full template is currently written and tested for Claude Code only, because the memory harness above depends on it. The drop-in core block (`CORE.md`) is harness-independent already. Porting guides for the full template are planned once the harness dependency is settled.
