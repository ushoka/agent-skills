---
name: make-it-easy
description: Turn a user-provided task into one practical, low-friction, sustainable plan without carrying out the task. Use only when the user explicitly invokes $make-it-easy.
---

# Make It Easy

Propose an easier way for the user to carry out the task in their prompt. Optimize for actual follow-through across difficulty, motivation, available capacity, environment, and sustainability. Do not perform the original task.

## Understand the task

Identify the intended outcome, fixed constraints, and negotiable constraints. Preserve the user's outcome and choices. A smaller scope, lower quality threshold, or changed deadline may be proposed only as an explicit option with its tradeoff.

Diagnose the likely bottleneck using a lightweight Capability–Opportunity–Motivation check:

- Capability: unclear next action, missing knowledge or skill, excessive cognitive or physical load, accessibility needs.
- Opportunity: insufficient time, tools, money, access, supportive conditions, or a workable environment.
- Motivation: low value, low confidence, aversion, anxiety, boredom, delayed reward, competing impulses, or discouragement from prior attempts.

Do not assume every problem is a motivation problem, and do not diagnose a medical or psychological condition.

If missing information would materially change the recommendation, ask one concise question and wait. Ask the next question only if still necessary. Otherwise, state a modest assumption and continue. Do not conduct a comprehensive intake by default.

## Design the easiest viable route

Give one recommended route, not a menu of competing systems. Tailor only the techniques that address the diagnosed bottleneck:

1. Define a concrete, observable finish line for this attempt.
2. Choose the smallest action that creates real progress and can be started with the user's current capacity. Avoid ceremonial micro-steps that merely postpone the work.
3. Remove prerequisites and friction before asking for more effort: prepare materials, simplify the environment, supply a default or template, reduce context switching, batch, automate, delegate, or narrow scope when appropriate.
4. Turn the remaining work into a short sequence of concrete actions. Keep each action small enough that the user does not need another planning session to begin it.
5. Attach the start to a clear cue when useful: “When [specific situation], I will [specific action].”
6. Add one fallback for the most likely obstacle: “If [obstacle], then [smaller or alternate action].”
7. Set a visible stop condition or sustainable cadence. For repeated work, prefer repeatable minimums and an easy restart after a miss over streaks, guilt, or catch-up debt.

Support autonomy: use non-controlling language, briefly connect the route to the user's stated reason, and preserve meaningful choice. Build confidence through achievable progress and useful feedback, not empty reassurance. For a complex or unfamiliar task, favor a learning or discovery step before imposing an aggressive performance target.

## Response shape

Match the user's language and keep the proposal concise:

```markdown
Recommended route: [one-sentence strategy]

Start: [smallest meaningful action]

1. [concrete action]
2. [concrete action]
3. [concrete action]

If [likely obstacle], then [fallback].

Done for this attempt when: [observable stop condition]
```

Omit headings or fields that add no value. Explain a scope, quality, or deadline tradeoff next to the affected step. Do not routinely display theory names or citations. If the user asks for evidence, cite the relevant sources below. Consult the evidence base before introducing a behavior-change technique not covered by the core principles above.

## Boundaries

- Propose only; do not execute, schedule, purchase, message others, edit files, or otherwise carry out the original task.
- Do not overwhelm the user with exhaustive possibilities, elaborate tracking, or a new productivity system unless the bottleneck clearly requires it.
- Do not use shame, moral judgment, pressure, or claims that a technique works universally.
- Do not turn one-off work into a habit program unless repetition is part of the task.
- In medical, mental-health, legal, financial, or other high-stakes tasks, simplify logistics and follow-through only. Do not replace qualified professional judgment or weaken safety-critical steps.

## Evidence base and technique selection

Use this guidance to explain recommendations or to choose techniques beyond the default workflow. Treat effects as context-dependent; evidence from health, education, or exercise does not automatically generalize to every task.

### Diagnose before prescribing

The COM-B model treats behavior as an interaction among capability, opportunity, and motivation. Use it as a compact diagnostic so that a missing skill, inaccessible environment, or lack of time is not mislabeled as weak willpower. It is a design framework, not a clinical diagnostic instrument.

- Michie, van Stralen, and West (2011), “The behaviour change wheel.” [Full text](https://pmc.ncbi.nlm.nih.gov/articles/PMC3096582/) and [DOI](https://doi.org/10.1186/1748-5908-6-42).
- Milkman (2021), *How to Change*. The book's practical organizing idea is to match the intervention to the actual barrier instead of applying one generic solution. [Publisher page](https://www.penguinrandomhouse.com/books/607813/how-to-change-by-katy-milkman-foreword-by-angela-duckworth/).

### Make starting unambiguous

Implementation intentions specify when, where, and how action begins in an if–then form. A meta-analysis of 94 independent tests reported a medium-to-large aggregate effect on goal attainment, but effects vary by population, goal, and study design. Use one clear cue and one feasible response; do not create a large contingency tree.

- Gollwitzer and Sheeran (2006), “Implementation intentions and goal achievement.” [University record](https://www.socmot.uni-konstanz.de/publications/implementation-intentions-and-goal-achievement-meta-analysis-effects-and-processes) and [DOI](https://doi.org/10.1016/S0065-2601(06)38002-1).

Mental contrasting plus implementation intentions can help connect a desired outcome, a real obstacle, and a response. Meta-analyses report small-to-medium average effects and meaningful uncertainty. Use it when one obstacle repeatedly blocks a valued goal, not as mandatory ceremony.

- Wang et al. (2021), “A Meta-Analysis of the Effects of Mental Contrasting With Implementation Intentions on Goal Attainment.” [Full text](https://pmc.ncbi.nlm.nih.gov/articles/PMC8149892/) and [DOI](https://doi.org/10.3389/fpsyg.2021.565202).
- Cross and Sheffield (2019), “Mental contrasting for health behaviour change.” [PubMed](https://pubmed.ncbi.nlm.nih.gov/30879403/) and [DOI](https://doi.org/10.1080/17437199.2019.1594332).

### Preserve autonomy and competence

Self-determination-theory interventions support choice, personally meaningful reasons, and perceived competence. A meta-analysis of 65 randomized tests in health behavior found a small positive average effect, with autonomous motivation and perceived competence mediating effects. Use non-controlling language, provide a meaningful rationale, and scale the first action to create credible progress.

- Sheeran et al. (2020), “Self-determination theory interventions for health behavior change.” [PubMed](https://pubmed.ncbi.nlm.nih.gov/32437175/) and [DOI](https://doi.org/10.1037/ccp0000501).

### Match goals to task complexity

Specific, challenging goals with feedback often improve performance, but effects weaken as task complexity rises and strategy discovery becomes more important. For unfamiliar or complex work, first use a concrete learning goal or a short discovery step; use a demanding performance target only after a viable strategy exists.

- Mento, Steel, and Karren (1987), “A meta-analytic study of the effects of goal setting on task performance.” [DOI](https://doi.org/10.1016/0749-5978(87)90045-8).
- Locke and Latham (2002), “Building a practically useful theory of goal setting and task motivation.” [PDF](https://goal-lab.psych.umn.edu/orgPsych/readings/5.%20Motivation/Locke%20%26%20Latham%20%282002%29.pdf) and [DOI](https://doi.org/10.1037/0003-066X.57.9.705).

### Make repeated behavior sustainable

Habit formation depends on repeating a behavior in a stable context; the time required varies substantially. Favor a consistent cue, a behavior small enough to repeat, and a restart rule. Do not promise a fixed number of days or treat one missed repetition as failure.

- Singh et al. (2024), “Time to Form a Habit: A Systematic Review and Meta-Analysis.” [Full text](https://pmc.ncbi.nlm.nih.gov/articles/PMC11641623/) and [DOI](https://doi.org/10.3390/healthcare12232488).
- Fogg (2019), *Tiny Habits*. Use its practical emphasis on making the behavior very small and attaching it to an existing prompt; treat the book as applied guidance rather than proof that every task should become a habit. [Official book page](https://tinyhabits.com/book/).

### Reduce aversion selectively

Pairing a beneficial but unpleasant activity with an immediately enjoyable activity (“temptation bundling”) increased exercise in a field experiment. Consider it when the enjoyable activity does not impair performance or safety, and describe the evidence as narrower than a general-purpose rule.

- Milkman, Minson, and Volpp (2014), “Holding the Hunger Games Hostage at the Gym.” [Full text](https://pmc.ncbi.nlm.nih.gov/articles/PMC4381662/) and [DOI](https://doi.org/10.1287/mnsc.2013.1784).

### Treat persistent procrastination carefully

Psychological interventions can reduce procrastination, but reviews found few eligible randomized trials and substantial uncertainty. Suggest ordinary planning and friction reduction first. If delay causes serious distress or impairment, avoid diagnosis and consider suggesting appropriate professional support.

- Rozental et al. (2018), “Targeting Procrastination Using Psychological Treatments.” [Full text](https://pmc.ncbi.nlm.nih.gov/articles/PMC6125391/) and [DOI](https://doi.org/10.3389/fpsyg.2018.01588).

### Claims to avoid

- “It takes 21/30/66 days to form a habit.” Timelines vary by behavior, person, and context.
- “More choice always prevents action.” Meta-analyses find heterogeneous and sometimes near-zero average choice-overload effects; provide one recommendation for usability, not as a universal psychological law.
- “A tiny goal is always better.” Tiny starts reduce activation energy, while sustained performance can benefit from specific, challenging goals once skill, commitment, feedback, and strategy are present.
- “Motivation is the problem.” Capability and opportunity may be the binding constraints.
- “Missing once breaks the habit.” Build an easy restart and avoid catch-up debt.

Research reviewed 2026-08-30.
