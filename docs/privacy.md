# Privacy

Plain answer: **nothing you share in your conversation comes to me, or to this project.**

## What actually happens

1. You copy a plain-text prompt from this repo.
2. You paste it into ChatGPT, Claude, or a similar AI tool — inside your own account with that provider.
3. You have a conversation with that AI.
4. The AI's provider (OpenAI, Anthropic, Google, etc.) handles that conversation under their own terms and privacy policies.

There is no backend server in this project. No analytics, no tracking, no telemetry. This repository is just a text file and some documentation. It cannot see your conversation.

## What your AI provider sees

Whatever you type into their chat interface goes through them. Each provider has their own privacy policy:
- [OpenAI](https://openai.com/policies/privacy-policy)
- [Anthropic](https://www.anthropic.com/legal/privacy)
- [Google](https://policies.google.com/privacy)

If you have concerns about what a provider does with your conversation, check their settings. Most offer "don't train on my data" toggles or temporary/incognito chat modes.

## About the Session Summary

At the end of a conversation, the advisor produces a Session Summary for you to save. That summary lives wherever you put it (a note on your phone, an email to yourself, a Google Doc). **This project has no access to it.**

The INTERNAL PROFILE block inside the summary is a working read the advisor builds to keep its tone consistent with you across sessions. It is advisor-facing, not a judgment. It's always in English (for cross-session stability) and contains short behavioral notes like "prefers short turns, analytical framing" — not diagnostic labels.

If you don't want to keep any state between sessions, just don't save the summary. Each conversation then runs fresh.

## Minors

Career Compass is intended for adults (18+). It's not designed for children.

## Updates

If this privacy note changes, it will change in the git history of this repository. You can always see the current version at its URL.
