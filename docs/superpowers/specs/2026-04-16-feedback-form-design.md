# Career Compass — Feedback Form Design Spec

## Problem

Career Compass has its first beta testers. We need a low-friction way to collect qualitative feedback, optimized for the psychology of job seekers (low self-efficacy, time-scarce, avoidance-prone). Audio-based input reduces effort vs. typing and yields richer context.

## Architecture

```
GitHub Pages (static)          Google Apps Script         Google Sheet
┌──────────────────┐           ┌───────────────┐         ┌──────────┐
│ /feedback/        │           │ doPost()      │         │ Feedback │
│   index.html      │──CTA──▶  │ - validates   │──────▶  │ - rows   │
│   form.html       │──POST──▶ │ - appends row │         │ per prompt│
└──────────────────┘           └───────────────┘         └──────────┘
```

- **Frontend**: Two static HTML pages deployed via existing GitHub Pages + Actions workflow
- **Transcription**: Web Speech API (browser-native, no API key, Chrome/Edge only)
- **Backend**: Google Apps Script web app — receives POST, appends to Google Sheet
- **Audio storage**: None. Only transcribed text is submitted
- **Fallback**: Non-Chrome browsers see a text area instead of the record button

## User Flow

### Page 1: Welcome (`/feedback/index.html`)

**Purpose**: Make the tester feel valued before asking for anything.

Content (Italian, "tu" register):
- Compass branding (🧭 or the existing favicon)
- Headline: "Grazie per aver provato Career Compass"
- Body: "La tua esperienza conta davvero. Quello che condividi qui mi aiuta a rendere questo strumento più utile per chi si trova nella tua stessa situazione."
- How it works box:
  - Ci vogliono circa 2-5 minuti
  - Puoi registrare la tua voce — niente da scrivere
  - Ti guido con alcune domande semplici
  - Non ci sono risposte giuste o sbagliate
- CTA button: "Inizia →"
- Footer note: "Il tuo feedback viene salvato come testo, non come audio."

**Psychological design choices:**
- "La tua esperienza conta davvero" — JSSE boost (your experience has value)
- "chi si trova nella tua stessa situazione" — SDT relatedness (you're helping people like you)
- "niente da scrivere" — reduces perceived effort upfront
- "non ci sono risposte giuste o sbagliate" — lowers performance anxiety

### Page 2: Feedback Form (`/feedback/form.html`)

#### Step 1: Track Selection

Two cards the user picks between:

- **Veloce** (~2 min): "Una domanda aperta sulla tua esperienza"
- **Approfondito** (~5 min): "Ti guido attraverso 4 domande"

On selection, the prompts for that track are previewed (visible but collapsed). User sees exactly what they'll be asked before committing.

#### Step 2: Guided Recording

After selecting a track, user sees:
- All prompts listed vertically, collapsed (showing only the question text)
- Progress bar at the top (e.g., "1 di 4")
- "Inizia" button to begin

Flow per prompt:
1. Current prompt expands, showing the question + a transcript area + record button
2. User clicks record → Web Speech API starts, real-time transcript appears
3. User clicks stop → transcript is editable in the text area
4. User clicks "Avanti" (next) → **response is POSTed immediately to Google Apps Script**
5. Current prompt collapses (showing a summary/checkmark), next prompt expands
6. Progress bar advances

**Partial submission**: Each prompt response is submitted individually as the user advances. If they leave after 2 of 4 prompts, those 2 responses are already saved. No data is lost.

**Session ID**: A random ID generated on page load, sent with each POST so partial responses from the same session are grouped in the Sheet.

#### Step 3: Review & Confirm

After the last prompt:
- All prompts shown expanded with their transcripts
- User can edit any transcript
- "Invia" (Send) button marks the session as complete
- If edits were made, the edited versions are re-POSTed

#### Step 4: Thank You

Confirmation screen:
- "Grazie! Il tuo feedback mi aiuta tantissimo."
- Optional: link back to Career Compass landing page

### Prompts

#### Veloce Track (1 prompt)

1. **"Com'è stata la tua esperienza con Career Compass? Racconta liberamente quello che vuoi."**
   (How was your experience with Career Compass? Share freely whatever you want.)

#### Approfondito Track (4 prompts)

1. **"Com'è stata la tua esperienza in generale?"**
   (How was your experience overall?)

2. **"C'è stato un momento in cui ti è sembrato particolarmente utile? E uno in cui non lo è stato?"**
   (Was there a moment where it felt particularly useful? And one where it didn't?)

3. **"C'è qualcosa che ti ha confuso o che non ha funzionato come ti aspettavi?"**
   (Was there anything that confused you or didn't work as you expected?)

4. **"Lo consiglieresti a qualcuno nella tua situazione? Perché?"**
   (Would you recommend it to someone in your situation? Why?)

## Google Sheet Structure

| Column | Description |
|--------|-------------|
| `session_id` | Random UUID, groups partial responses |
| `timestamp` | ISO timestamp of submission |
| `track` | `veloce` or `approfondito` |
| `prompt_index` | 1-based index of the prompt |
| `prompt_text` | The question that was asked |
| `response` | Transcribed/typed response text |
| `is_final` | `true` if submitted via the review screen, `false` if partial |
| `input_method` | `voice` or `text` (whether they used recording or typed) |
| `user_agent` | Browser user agent string |

One row per prompt response. A complete "approfondito" session = 4 rows with the same `session_id`.

## Google Apps Script

A simple `doPost(e)` web app that:
1. Parses the JSON body
2. Validates required fields (session_id, response)
3. Appends a row to the Sheet
4. Returns `{ "status": "ok" }`

Deployed as a web app with "Anyone" access (no Google sign-in required for submitters).

## Prompt Integration

Add to `prompt.md` — a new rule for when to surface the feedback link:

```
### Feedback
When the session is ending naturally (user wraps up, completes a module, or
expresses that they found something useful), mention the feedback page:

"Se vuoi, puoi condividere la tua esperienza qui: [FEEDBACK_URL].
Mi aiuta tantissimo a migliorare questo strumento."

NEVER surface the feedback link when:
- The user is showing confidence collapse signals (Rule 8)
- The user is frustrated or expressing that the tool isn't helping
- Mid-flow when the user is actively working on a task
```

## Technical Constraints

- **Web Speech API**: Chrome/Edge only (~75% of users). For unsupported browsers: hide the record button, show only the text area with a note "Il tuo browser non supporta la registrazione vocale, ma puoi scrivere il tuo feedback qui."
- **Google Apps Script**: Free tier, no rate limits for this scale. Cold start may add 1-2s to first POST.
- **No authentication**: Anyone with the link can submit. Acceptable for beta scale. If spam becomes a problem later, add a simple token in the URL.
- **Language**: Italian only for now. The page structure supports adding languages later but it's not in scope.

## Out of Scope

- Multi-language support (Italian only for beta)
- User authentication / login
- Audio file storage
- Analytics dashboard (Gianni reads the Sheet directly)
- A/B testing of prompt wording
- Mobile-native recording (Web Speech API works on mobile Chrome)

## Success Criteria

- A beta tester can go from link → recorded feedback → submitted in under 3 minutes (veloce track)
- Partial responses are captured even if the user abandons mid-flow
- Gianni receives structured, readable feedback in a Google Sheet
- The experience feels warm and respectful, not like a survey
