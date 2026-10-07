## Project memory (Epistemic Memory, core)

You lose all context between sessions. This project keeps its memory in plain markdown files under `convos/` (create the folder if it's missing), so that the next session can continue where you stopped, and so that anyone reading later can tell what was actually decided and what was only proposed or guessed.

### Files

Start a topic for work that spans sessions, needs a decision from the user, or produces results worth keeping. A small fix finished in one session needs no topic. Each topic gets two files in `convos/`, named with the start date in `YY-MM-DD` format and a short slug, e.g. `26-10-07_cache_redesign_LOG.md`:

- `{date}_{topic}_LOG.md` is the chronological record. New entries go at the bottom, separated by `---`, each starting with the date, your model name, and a short title that is unique within the file. Never change or remove existing text, even when it turned out wrong; dead ends and corrections stay in. The only insertions allowed above the bottom are answers to the user's comments and `OUTDATED:` notes (both described below).
- `{date}_{topic}_SUMMARY.md` is the current state in a page or less, with a `**Last updated:**` line near the top. It has no contradictions; write "we first tried X, then switched to Y because Z". Update it whenever the LOG changes something important. It is what the next session reads first.

At the start of a session, list `convos/`, read the SUMMARYs related to the task, and look for `C:` comments that have no answer beneath them yet (see below). Read a LOG only when its SUMMARY isn't enough, and read the whole LOG when the user points you at it. Put analyses, plans and results into the LOG, not only into chat.

### Status markers

Tag the decisions, results, conclusions and assumptions that later work depends on, so that nobody mistakes a guess for a decision:

- `[decided]` means the user explicitly agreed. Only the user can make something decided, and the user's words must be on file: a `C:` comment, or their chat message transcribed as described below. Your own proposals and implementations stay `[concluded]` until then.
- `[verified]` means something was tested or observed. Say what, when and how much, e.g. `[verified: full test suite 26-10-07, 42/42 pass]`.
- `[concluded]` means reasoned but not approved. Put the reasoning next to it.
- `[assumed]` is a working assumption that may need revisiting.
- `[superseded]` means replaced by a later decision. Point to the replacement.
- `OUTDATED: <reason>` is a note inserted next to LOG text that turned out wrong; since the LOG is append-only, this is how you correct it.

Don't describe a proposal as a plan, or a plan as done. Later sessions will take your wording at face value.

### Numbers

Never write a specific number (a result, count, percentage, measurement) from memory. Re-read the source file or re-run the command first, and say what the number means: compared to what, and with which caveats. Be most careful when you attach a source, because a remembered number with a citation looks checked even though it wasn't.

### The user's comments (optional for the user)

The user may comment inside files in `convos/`: a line starting with `C:` (possibly indented) is the user's comment, usually placed under the paragraph it refers to. Answer directly beneath it, starting with `Answer (date, model name):`, so that question and answer stay together. If your answer changes a plan or conclusion, also add a LOG entry at the bottom and update the SUMMARY. A question from the user is not an approval.

Never start a line with `C:` yourself, with one exception. When the user approves, rejects or corrects something in chat, add a LOG entry at the bottom that says what it refers to (e.g. "option 2 from the entry *cache plan*: keep the in-memory cache"), followed by their words verbatim as `C: (transcribed from chat) <their words>`. Then update the tags and the SUMMARY. If it's unclear which proposal they mean, ask.

Commenting is optional. Going back to a paragraph and commenting on it is often easier than referring to an old chat from memory, but the user can always just chat with you instead.
