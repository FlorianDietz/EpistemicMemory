# Research Heuristics

Read when you plan a research experiment or a research effort that spans several runs or sessions, and again before you write up results for the user.

**These are heuristics.** They come from a single research project (seven runs on one idea), where each of them would have saved time or prevented a wrong turn at least once. Use them as questions to ask yourself, and skip one when the situation clearly differs. Other kinds of research, and future models, may fail in other ways. If one misfires or turns out useless, record that in FEEDBACK.md so the list can be pruned.

## At kickoff

- **Search the problem, not the analogy.** Restate the idea in the standard vocabulary of the field it belongs to, and search that field's existing methods and strongest baselines, not only work that resembles your mechanism or what inspired it. (Example: a brain-inspired "per-neuron learning-rate spread" is, in machine-learning terms, a multi-task-optimization and step-size-adaptation problem. Those literatures held the method that eventually won and the baselines that mattered, and they were found only after several runs.)
- **Check the premise on real data first.** If the value of the idea depends on "real cases have property X", measure X on one real case before building synthetic tasks that have X by construction.
- **Write kill criteria and a round budget.** Say which result ends the line of work, and after how many rounds you will step back and ask whether to continue.
- **Write the email first.** Write the one-sentence claim and the reply you would expect from the target reader (a skeptical stranger who doesn't know the author). Revisit both whenever you report results. If the expected reply is "huh, a toy", say what would change it.

## During runs

- **Baseline parity.** The baseline gets as many tuned settings as the method (optimizer settings, schedule, loss weights). Run the field's most dangerous baseline early, not last.
- **Define a control by the variable it changes.** Naming a method instead is risky: a named method brings its own side effects. Before using one as a control, check whether an earlier result already shows it failing for an unrelated reason.
- **Probe before predicting.** For the behavior of coupled systems (tasks sharing weights, interacting optimizers), a probe of the extreme settings that takes minutes beats reasoning about them. Settings that are best for a component on its own often don't carry over to the joint system. Keep a tally of predictions made in advance against outcomes; a low hit rate means "probe more, reason less" for that kind of system.
- **Don't stop to ask about steps the user has already authorized.** If the next step is free, reversible, and within what the user has already allowed (e.g. "CPU experiments are free, run what you think is sensible"), do it and report it. Save questions for goals, the bar a result must clear, spending, and claims.

## Before writing up results

- **Lead with the decision.** Open with about five plain lines: what we learned, what it means for the goal, the decision needed, and your recommendation. No agent-coined terms in those lines. Evidence goes below.
- **A negative result needs its boundary.** Before recommending "stop", report the strongest alternative explanation, the setting most likely to flip the result, and how the setup differs from the theory that predicted success.
- **Ask the reviewer's questions yourself.** Would a stranger receiving this care? What else could explain it? Where does it work, and why does the theory fail here? What does the literature already know? Should we still be doing this at all?
