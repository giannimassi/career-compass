# How Career Compass Works

A look inside the prompt for the curious.

## The core idea

Modern AI chat tools are excellent conversational partners but default to generic, checklist-style advice when you ask about your career. Career Compass is a single paste-in prompt that shapes the model's behavior so it acts like a **structured career advisor** — one that figures out where you are before it suggests anything.

## The structure

The prompt does five things:

### 1. Sets a consistent advisor persona

Direct, warm, confident, curious. No hollow affirmations ("great question!"), no AI self-disclosure, no therapy-speak. Short sentences. One question per turn.

### 2. Identifies what stage you're in

Before giving any advice, the advisor places you in one of five stages:

| Stage | What it means |
|---|---|
| Exploring | No clear direction. You know something needs to change but not what |
| Orienting | A general sense of direction, not committed to a target |
| Targeting | Know what you want, pursuing specific roles |
| Executing | Have active interviews or offers |
| Stuck (overlay) | Was executing, now stalled — applications go nowhere |

The advice you need is different at each stage. This is the single most common failure mode of generic career advice: tactical polish (how to write a cover letter) given to someone who doesn't know what they want yet.

### 3. Runs the right **modules** for your stage

Each stage has a sequence of focused modules, each producing a concrete artifact you can use:

- Values & constraints → what's non-negotiable
- Direction setting → a one-sentence "north star"
- Positioning → a 2–3 sentence pitch
- Role sourcing → target companies and where to look
- LinkedIn audit → specific profile changes
- CV review → specific edits (we don't rewrite — more on that below)
- Application strategy → A/B/long-shot tiers with weekly cadence
- Interview prep → behavioral story bank
- Negotiation prep → anchor, floor, BATNA, non-salary levers
- Breakdown diagnostic → when you're stuck, which funnel zone is the problem

### 4. Reads your communication style silently

Throughout the conversation, the advisor forms a quiet working read of how you communicate — pace, directness, what motivates you, how you handle stress — and adjusts its tone accordingly.

**This is silent.** The advisor never psychoanalyzes back to you, never quotes the framework, never tells you "I notice you're showing X pattern." If you ask what it thinks of you, it declines, because the point is to be useful, not to label you. The read exists to make the conversation feel like it's with a colleague who knows you, not with a script.

### 5. Saves state between conversations

At the end of each session, the advisor produces a **Session Summary** — a structured block you can copy, save somewhere, and paste at the start of your next session. That lets you pick up where you left off without losing context, even if you come back a month later.

## What's evidence-based

The inference rules inside the prompt are drawn from a 2026 synthesis of empirical research on adult job-search failure modes. 16 IF/THEN patterns are encoded — for example:

- If you've submitted 20+ applications with zero interview callbacks, the bottleneck is positioning (usually CV format), not volume. Specific research: a UK field experiment (n≈9,022) showing that reformatting a CV by "years worked" instead of dates increased callbacks by 8–15%.
- If you want the advisor to "generate 50 cover letters," it will refuse — because mass-apply worsens outcomes through ATS spam filters and recruiter ghosting.
- If you've been unemployed 8+ months, strategy changes are no longer optional — duration stigma worsens callbacks independent of effort.

The full research synthesis is in [research/failure-modes.md](../research/failure-modes.md) if you want to see the sources.

## What the advisor **won't** do

- **Rewrite your CV.** It reviews and proposes changes. You make the edits. This is deliberate — AI-rewritten CVs fail in interviews when you can't defend every claim.
- **Automate applications.** Mass-apply tools look efficient; they're a statistical trap.
- **Psychoanalyze you.** The communication-style inference is silent by design.
- **Pretend to be human.** If you ask directly, it confirms it's an AI.
- **Push through a collapsed-confidence state with more strategy.** If you're in a "what's the point" place, it pauses strategy and routes to small-wins and social accountability first.

## Multi-language

The advisor matches the language you write in. For Italian users, it also loads cultural calibration — register adjustments (more indirect/relational baseline), tactical adjustments (prioritizing passaparola over LinkedIn cold outreach, CCNL-aware negotiation framing), and regional skill matching.

## Multi-session use

The Session Summary at the end of each session is the memory layer. Save it in a note, email to yourself, keep it in a Google Doc — wherever is easy. When you start the next session, paste the whole thing as your first message and the advisor picks up with full context.

## Modular loading (for the technically curious)

The prompt is **thin** (~190 lines). Extended material — full module definitions, the 16 inference rules, Italian calibration, the AI-tool playbook — lives in addon files in this repo. The prompt includes a URL to an index file that the AI model fetches on demand when it needs depth.

If your AI tool has web search disabled, the thin prompt still works — it has everything essential. The addons add depth but aren't required.

This keeps the paste-in prompt small enough to work on mobile, copy cleanly, and load fast.

## Credits

Built in April 2026 by [Gianni Massi](https://github.com/giannimassi). Research synthesis informed by ChatGPT Deep Research and Google Gemini Deep Research (April 2026).
