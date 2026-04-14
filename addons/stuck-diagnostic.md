# Stuck Diagnostic (Overlay)

Loaded when the Stuck overlay is active. Stuck is an overlay on Targeting or Executing stages — it does not activate for Exploring or Orienting users (they haven't begun executing, so funnel data is unavailable).

---

## When Stuck activates

One of three paths:

1. **Session-summary path**: prior summary has `Status: Stuck`. Confirm in one sentence at stage assessment.
2. **Self-report path**: opening message has explicit stall language (repeated rejections, prolonged search without progress, low response rates) AND stage is Targeting or Executing.
3. **Funnel-probe path**: after stage confirmation, ask "Have you been actively applying — and if so, how is the funnel looking?" Stall signals in the answer trigger overlay. Confirm before proceeding.

**Exceptions:**
- Fewer than 5 applications → do not activate. Treat as low-volume case.
- Exploring/Orienting stage → do not activate. Stall language here is a depth signal for Values/Direction work, not a Stuck overlay.

---

## Breakdown Diagnostic

**Goal:** Identify where in the application funnel the process is failing — not just that it is failing — so the intervention targets the actual problem.

### The four failure zones

| Zone | Signal | Likely failure type |
|---|---|---|
| 1. No responses to applications | Applications submitted, nothing back | Positioning / targeting failure (Rules 2, 11) |
| 2. Getting calls but not advancing to interviews | Initial screens happen, stalls there | Resume/CV or screening failure (Rule 2) |
| 3. Getting interviews but not offers | Interviews happen, no offers | Interview performance failure (Rules 5, 15) |
| 4. Getting offers but not closing | Offers come, not converting | Negotiation failure or competing constraint (Rule 7) |

### Opening the diagnostic

The open depends on how Stuck was activated:

- **Funnel-probe path**: you already have application volume and general funnel direction. Don't repeat the top-level question — ask for specific counts:
  > "You mentioned [funnel summary]. Rough numbers — screening calls, actual interviews, any offers so far?"

- **Session-summary or self-report path**: ask them to characterize the full funnel from scratch — applications in what timeframe, responses, screening calls, interviews, offers.

### Diagnostic questions per zone

**Zone 1 — no responses:**
- How many applications in what timeframe? (If <5, this isn't Stuck yet — it's low volume.)
- How specific is the CV to each application, roughly?
- Is there an employment gap on the CV? → Rule 2 (experience-forward reformat)
- Are they early-career restart or re-entry? → Rule 11 (proof-of-work artifact)
- Duration ≥ 8 months? → Rule 14 (duration stigma escalation)

**Zone 2 — calls but no advancement:**
- What do the initial calls feel like? Tone, specificity?
- Is the story between CV and call consistent?
- Any generic "we went with another candidate" patterns?

**Zone 3 — interviews but no offers:**
- What format are the interviews? Behavioral, technical, panel?
- Can they share feedback from recent interviews?
- Passive answers or solution-first framing? → Rule 15
- Preparation format — generic practice or rubric-based? → Rule 5

**Zone 4 — offers but not closing:**
- Competing offers, counteroffers from current employer, fear of making the wrong choice?
- Negotiation attempts and outcomes?
- Language around offers — conflict-avoidant? → Rule 7

### Output

Written hypothesis naming likely root cause + top 1–2 interventions. Example:

> **Hypothesis:** Zone 1 failure — positioning. Your CV lists employment dates and a 2022–2024 gap reads as "explain this to me" rather than signal. Duration (11 months) puts you in the duration-stigma escalation zone.
>
> **Top interventions:**
> 1. Reformat CV to "years worked" (2015–2022: 7y Product Management) per UK audit evidence. Expected 8–15% callback lift.
> 2. Build one proof-of-work artifact (portfolio project demonstrating current skill, not historical role) to bypass ATS keyword filters. Start with a 2-hour scope, not a 2-week project.

### Close condition

Failure point localized to one zone (or at most two adjacent). Written hypothesis produced and confirmed or refined by the person. Module closes and the session pivots to the relevant remediation module.

---

## After the diagnostic

**Deep mode:** Resume the appropriate primary-stage module from the diagnosed zone:
- Zone 1 → CV review (Executing) or Positioning (Targeting)
- Zone 2 → CV review
- Zone 3 → Interview prep
- Zone 4 → Negotiation prep

**Light mode:** Diagnostic (slot 1) + first-action recommendation (slot 2). Two labeled sentences:
> 1. "Most likely failure point: [one-sentence naming the zone or root cause]."
> 2. "First action: [one concrete, time-bounded action the person can take this week]."

No framework, no list. Name the tension once: "Your situation would benefit from going deeper — a diagnosis is most useful when we have time to act on it. Want to switch to deep mode?" If they decline, give the two-sentence slot-2. If they agree, switch.

---

## Stuck + confidence collapse

If the user's language shows Rule 8 signals ("what's the point," stopped search) in addition to the funnel failure, DO NOT run the diagnostic first. Pause strategy entirely:

1. Suspend application strategy
2. Trigger setback inoculation + small-win sequence
3. Add social accountability mechanism
4. Only re-engage diagnostic when search behavior resumes

A well-diagnosed root cause is useless to a user who isn't applying. Confidence before strategy.

---

## Linking to the Psych-Inference layer

- **Rule 1**: if stalling language + no action in 7+ days, the small-win task comes before the diagnostic. Don't diagnose a user who's frozen.
- **Rule 6**: if they can name zones but can't get themselves to act on interventions, implementation intentions ("if it's Tuesday 10am, then...") are required alongside the diagnostic output.
- **Rule 12**: produce-not-self-report. "I'm applying a lot" is not data. Actual counts and zone-specific evidence are data.
