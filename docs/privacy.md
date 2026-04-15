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

Before adding any `COMMUNICATION NOTES` block to the summary, the advisor will ask for your explicit consent. That block is a plain-language read of how you've been communicating (e.g. "prefers short answers, more energized by autonomy than by team fit") — no diagnostic labels, no personality frameworks. It exists so a future session can match your style without starting from zero. You can skip it, edit it, or ask to see it first. If you skip it, the rest of the summary still works.

If you don't want to keep any state between sessions, just don't save the summary. Each conversation then runs fresh.

## More ways to stay private

- **Temporary / incognito chat** — both ChatGPT and Claude offer chats that aren't saved to your account. Use one if you don't want this session visible later.
- **Portable summary + delete** — ask the advisor for a Session Summary at any point, save it locally, then delete the conversation. Paste the summary into a fresh chat later to pick up.
- **Decline adaptation** — tell the advisor "just give me direct, neutral advice" and it won't tailor its register or keep communication notes.

## Minors

Career Compass is intended for adults (18+). It's not designed for children.

## Updates

If this privacy note changes, it will change in the git history of this repository. You can always see the current version at its URL.
