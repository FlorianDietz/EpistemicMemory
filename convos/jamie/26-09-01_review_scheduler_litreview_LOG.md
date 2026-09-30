# Literature review: spaced-repetition scheduling for the flashcard app

> **FICTIONAL DEMONSTRATION CONTENT.** Everything in this file is invented — the user "jamie", the prior projects, the papers, the results. It exists to show the conventions in action: entry headers, in-place `C:` answers (dated and attributed), epistemic status markers, the chat-transcription marker for ratifications, append-only corrections, and the paired SUMMARY. Delete `convos/jamie/` when setting up a real project.

I want to replace the fixed review intervals in the flashcard app with a proper spaced-repetition scheduler. Please review what's out there and recommend an algorithm. Check the earlier projects first so we don't redo work.

---

26-09-01, Claude Opus 4.5 — *scheduler survey and recommendation*

I have read the README index and the SUMMARYs of the two related prior topics: the retention-metrics analysis (26-07-12, defined our forgetting-curve fit and the `recall_prob` metric) and the review-queue refactor (26-08-03, which made the scheduler pluggable — the interface we'd implement against).

Building on the retention-metrics analysis: our own data showed recall probability dropping faster for cloze cards than for pair cards, so whatever we pick should support per-card-type parameters.

Papers reviewed (4, from the last three years):

- **Vasquez & Lindqvist 2024, "Half-Life Regression Revisited"** — refits half-life regression on 12M reviews; strong baselines but assumes server-side training, which we don't have.
- **Okafor et al. 2025, "Lightweight Adaptive Scheduling on Device"** — closest to our constraints; pure on-device updates, no training pipeline. Reported 11% retention gain over fixed intervals — note this is against a *fixed-interval* baseline, so it's an upper bound on what we'd gain over our current setup, which is already semi-adaptive.
- **Bruckner 2023, "Optimal Spacing Is Not Robust Spacing"** — argues optimal-per-user schedules degrade badly under irregular usage; recommends bounded adjustment steps. Directly relevant: our users skip days often.
- **Tanaka & Osei 2025, "Cloud-Personalized Review Scheduling"** — best reported numbers of the four, but requires streaming review events to a service for model updates.

Deepest look at Okafor et al., since it fits our constraints best: the update rule is a small multiplicative adjustment per review outcome, with per-category multipliers — which covers the cloze-vs-pair difference from our retention analysis. Their evaluation caveat: participants were university students, denser review habits than our users [assumed: this transfers anyway, since Bruckner's bounded-step argument suggests the method degrades gracefully].

[concluded] Recommendation: implement the Okafor-style on-device rule with Bruckner's bounded adjustment steps, per-card-type multipliers seeded from our own forgetting-curve fits. Tanaka & Osei's approach would likely score higher but needs a cloud component.
    C: Why not just take Tanaka & Osei's numbers if they're the best? What would the cloud part actually cost us?
    Answer (26-09-01, Claude Opus 4.5): Their gains come from a server-side model retrained on streamed review events. For us that means running and paying for that service, plus every review event leaving the device. The paper's own ablation (Table 4) shows the on-device variant loses about half the gain — that remaining gain is real, but it buys us a privacy change and an ops dependency the app currently doesn't have. My read: not worth it at our scale; revisit if retention becomes the bottleneck.
    OUTDATED: the offline/privacy requirement stated in the next section rules the cloud direction out permanently — there is no revisit path. See the entry "*offline constraint folded in; recommendation ratified*".

---

The app has to keep working fully offline, that's a hard requirement I should have stated earlier. Also: our privacy policy says review data never leaves the device, so the Tanaka & Osei direction is out entirely, not just deprioritized.

---

26-09-02, Claude Opus 4.5 — *offline constraint folded in; recommendation ratified*; I have read this LOG and the two prior SUMMARYs, not the papers themselves.

Noted — the offline/privacy requirement rules out any cloud-dependent variant permanently, not as a tradeoff. The comparison to Tanaka & Osei in yesterday's entry stands as analysis, but its "revisit if retention becomes the bottleneck" conclusion is [superseded]: there is no revisit path while the privacy policy holds. I have placed an OUTDATED marker next to that sentence in yesterday's entry, so a reader who stops there doesn't act on it.

The recommendation itself is unchanged, since it was already fully on-device. Jamie confirmed in chat; transcribing verbatim so the ratification is on the record rather than resting on my paraphrase:
    C: (transcribed from chat) yes, go with that
With that on file it is now [decided]: Okafor-style on-device updates, bounded steps per Bruckner, per-card-type multipliers seeded from our forgetting-curve fits. Implementation goes against the pluggable scheduler interface from the review-queue refactor; I'll start it as a separate topic.
