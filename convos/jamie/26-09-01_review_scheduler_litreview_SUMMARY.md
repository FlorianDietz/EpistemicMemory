# SUMMARY: Spaced-repetition scheduler literature review

> **FICTIONAL DEMONSTRATION CONTENT** — see the note at the top of the LOG. Delete `convos/jamie/` when setting up a real project.

**Last updated:** 26-09-02 (offline constraint added; recommendation ratified)

## Outcome

[decided] The flashcard app's scheduler will use on-device adaptive updates in the style of Okafor et al. 2025 ("Lightweight Adaptive Scheduling on Device"): multiplicative per-review adjustments, bounded step sizes per Bruckner 2023 (robustness under irregular usage — our users skip days), and per-card-type multipliers seeded from our own forgetting-curve fits (cloze cards decay faster than pair cards, per the retention-metrics topic).

A hard constraint arrived after the initial review: the app must work fully offline, and the privacy policy forbids review data leaving the device. This permanently rules out the cloud-personalized direction (Tanaka & Osei 2025), which had the best reported numbers. We initially treated it as merely deprioritized ("revisit if retention becomes the bottleneck"); that is superseded — there is no revisit path while the privacy policy holds. Details in the LOG entry "*offline constraint folded in; recommendation ratified*".

## Caveats worth remembering

- Okafor et al.'s 11% retention gain is measured against a fixed-interval baseline; our current setup is already semi-adaptive, so expect less.
- [assumed] Their student-population results transfer to our sparser-usage users; Bruckner's bounded-step argument suggests graceful degradation, but we have not verified this on our data.

## Next step

Implement against the pluggable scheduler interface from the review-queue refactor (26-08-03 topic), as a new topic.

## Source files

- LOG: `convos/jamie/26-09-01_review_scheduler_litreview_LOG.md`
- See also: `convos/jamie/26-07-12_retention_metrics_SUMMARY.md` (fictional) — forgetting-curve fits used to seed the multipliers
- See also: `convos/jamie/26-08-03_review_queue_refactor_SUMMARY.md` (fictional) — the scheduler interface this will be implemented against
