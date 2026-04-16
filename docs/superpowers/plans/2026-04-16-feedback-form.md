# Feedback Form Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add an audio-first feedback form to Career Compass so beta testers can record voice feedback that gets transcribed and submitted to a Google Sheet.

**Architecture:** Two static HTML pages (`/feedback/` welcome + `/feedback/form`) on existing GitHub Pages. Web Speech API for client-side transcription. Google Apps Script as a POST endpoint that writes to Google Sheets. Each prompt response is submitted individually for partial-completion resilience. Prompt updated to surface the feedback link at natural session-end moments.

**Tech Stack:** Vanilla HTML/CSS/JS (matching existing site), Web Speech API, Google Apps Script, Google Sheets

**Spec:** `docs/superpowers/specs/2026-04-16-feedback-form-design.md`

---

## Human Gates (do these first)

### Gate 0: Create Google Sheet + Apps Script

**You need to do this manually — it requires your Google account.**

1. Go to https://sheets.google.com → Create new spreadsheet
2. Name it "Career Compass Feedback"
3. In row 1, add these headers:

```
session_id | timestamp | track | prompt_index | prompt_text | response | is_final | input_method | user_agent
```

4. Go to **Extensions → Apps Script**
5. Replace the default code with this:

```javascript
function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  var data = JSON.parse(e.postData.contents);

  sheet.appendRow([
    data.session_id || "",
    data.timestamp || new Date().toISOString(),
    data.track || "",
    data.prompt_index || "",
    data.prompt_text || "",
    data.response || "",
    data.is_final || false,
    data.input_method || "text",
    data.user_agent || ""
  ]);

  return ContentService
    .createTextOutput(JSON.stringify({ status: "ok" }))
    .setMimeType(ContentService.MimeType.JSON);
}
```

6. Click **Deploy → New deployment**
7. Type = **Web app**
8. Execute as = **Me**
9. Who has access = **Anyone**
10. Click **Deploy** → Copy the URL (looks like `https://script.google.com/macros/s/.../exec`)
11. **Give me this URL** — I'll wire it into the form HTML.

---

## File Structure

```
site/
├── index.html                    # existing — add "Dai il tuo feedback" link
├── favicon.svg                   # existing — unchanged
├── feedback/
│   ├── index.html                # NEW — welcome page
│   └── form.html                 # NEW — recording + submission form
└── prompt.md                     # copied by CI — modify source prompt.md at repo root
prompt.md                         # existing — add feedback section
```

---

## Task 1: Welcome Page

**Files:**
- Create: `site/feedback/index.html`

- [ ] **Step 1: Create the welcome page HTML**

Create `site/feedback/index.html` — a standalone HTML page that reuses the Career Compass visual style (Fraunces font, warm/earthy color palette from `site/index.html` CSS variables). Content in Italian:

```html
<!doctype html>
<html lang="it">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<meta name="robots" content="noindex">
<title>Feedback · Career Compass</title>
<link rel="icon" type="image/svg+xml" href="../favicon.svg">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,400;9..144,500;9..144,700;9..144,900&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #fbf7ee;
    --bg-soft: #f4efe0;
    --card: #ffffff;
    --ink: #1a1a1a;
    --ink-soft: #5a5246;
    --ink-softer: #7a7266;
    --accent: #2b5a4a;
    --accent-ink: #fff;
    --accent-soft: #e9f0eb;
    --warm: #d4805c;
    --warm-soft: #f7e6dc;
    --line: #e6dfce;
    --shadow: 0 1px 2px rgba(0,0,0,0.04), 0 8px 24px rgba(42,30,10,0.06);
  }
  @media (prefers-color-scheme: dark) {
    :root {
      --bg: #15130f;
      --bg-soft: #1f1c17;
      --card: #1f1c17;
      --ink: #f3ede0;
      --ink-soft: #b8b1a0;
      --ink-softer: #8a8478;
      --accent: #6fbb9b;
      --accent-ink: #0b1a15;
      --accent-soft: #1d2a25;
      --warm: #e89778;
      --warm-soft: #2d2319;
      --line: #2d2923;
      --shadow: 0 1px 2px rgba(0,0,0,0.5), 0 12px 32px rgba(0,0,0,0.35);
    }
  }
  * { box-sizing: border-box; margin: 0; }
  body {
    font-family: system-ui, -apple-system, sans-serif;
    background: var(--bg);
    color: var(--ink);
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 2rem;
  }
  .container {
    max-width: 520px;
    text-align: center;
  }
  .compass { font-size: 3rem; margin-bottom: 1rem; }
  h1 {
    font-family: 'Fraunces', serif;
    font-size: 1.6rem;
    font-weight: 700;
    margin-bottom: 0.75rem;
    color: var(--ink);
  }
  .subtitle {
    color: var(--ink-soft);
    line-height: 1.6;
    margin-bottom: 1.5rem;
    font-size: 0.95rem;
  }
  .info-box {
    background: var(--accent-soft);
    border-radius: 12px;
    padding: 1.25rem;
    text-align: left;
    margin-bottom: 1.5rem;
  }
  .info-box h2 {
    font-size: 0.9rem;
    font-weight: 600;
    margin-bottom: 0.5rem;
    color: var(--ink);
  }
  .info-box ul {
    padding-left: 1.2rem;
    color: var(--ink-soft);
    font-size: 0.88rem;
    line-height: 1.7;
  }
  .info-box strong { color: var(--ink); }
  .cta {
    display: inline-block;
    background: var(--accent);
    color: var(--accent-ink);
    border: none;
    padding: 0.85rem 2.5rem;
    border-radius: 8px;
    font-size: 1rem;
    font-weight: 500;
    cursor: pointer;
    text-decoration: none;
    transition: opacity 0.15s;
  }
  .cta:hover { opacity: 0.9; }
  .footnote {
    color: var(--ink-softer);
    font-size: 0.78rem;
    margin-top: 1rem;
  }
</style>
</head>
<body>
<div class="container">
  <div class="compass">🧭</div>
  <h1>Grazie per aver provato Career Compass</h1>
  <p class="subtitle">
    La tua esperienza conta davvero. Quello che condividi qui mi aiuta a rendere
    questo strumento più utile per chi si trova nella tua stessa situazione.
  </p>
  <div class="info-box">
    <h2>Come funziona:</h2>
    <ul>
      <li>Ci vogliono <strong>circa 2–5 minuti</strong></li>
      <li>Puoi <strong>registrare la tua voce</strong> — niente da scrivere</li>
      <li>Ti guido con alcune domande semplici</li>
      <li>Non ci sono risposte giuste o sbagliate</li>
    </ul>
  </div>
  <a class="cta" href="./form.html">Inizia →</a>
  <p class="footnote">Il tuo feedback viene salvato come testo, non come audio.</p>
</div>
</body>
</html>
```

- [ ] **Step 2: Verify the page renders**

Open `site/feedback/index.html` in a browser. Check:
- Dark/light mode both work
- CTA links to `./form.html`
- Text is readable and warm

- [ ] **Step 3: Commit**

```bash
git add site/feedback/index.html
git commit -m "feat(feedback): welcome page — warm Italian copy with JSSE/relatedness framing"
```

---

## Task 2: Feedback Form Page — HTML/CSS Shell

**Files:**
- Create: `site/feedback/form.html`

- [ ] **Step 1: Create the form page with track selection + prompt UI**

Create `site/feedback/form.html` — the main form page. This step creates the full HTML/CSS structure with the track selection cards, collapsible prompt sections, progress bar, record button, transcript areas, and review/submit flow. All interactive behavior will be wired in the next tasks.

The page structure:
1. **Track selection** — two cards (Veloce / Approfondito), clicking one shows the prompt preview
2. **Prompt list** — all prompts for the selected track, collapsed, with question text visible
3. **Active prompt** — expanded state with record button + transcript textarea
4. **Progress bar** — shows completion (e.g., "2 di 4")
5. **Review screen** — all prompts expanded with edit capability + "Invia" button
6. **Thank you screen** — confirmation after submit

Key CSS considerations:
- Reuse exact same CSS variables and Fraunces font from welcome page
- Collapsible prompts: use a `.prompt-card` class with `.prompt-card.active` for expanded state
- Record button: prominent, round, with a pulsing animation when recording
- Progress bar: thin bar at top of prompt section
- Smooth transitions for expand/collapse

Prompts data (hardcoded in JS):

```javascript
var PROMPTS = {
  veloce: [
    { text: "Com'è stata la tua esperienza con Career Compass? Racconta liberamente quello che vuoi." }
  ],
  approfondito: [
    { text: "Com'è stata la tua esperienza in generale?" },
    { text: "C'è stato un momento in cui ti è sembrato particolarmente utile? E uno in cui non lo è stato?" },
    { text: "C'è qualcosa che ti ha confuso o che non ha funzionato come ti aspettavi?" },
    { text: "Lo consiglieresti a qualcuno nella tua situazione? Perché?" }
  ]
};
```

Create the full HTML file with all CSS inline (same pattern as the landing page). Include placeholder `<script>` tags — the JS behavior comes in Tasks 3-5.

- [ ] **Step 2: Verify the static layout**

Open `site/feedback/form.html` in a browser. Check:
- Track selection cards render and are clickable (no JS wired yet, just visual)
- Color scheme matches the welcome page
- Dark/light mode works
- Mobile responsive (test at 375px width)

- [ ] **Step 3: Commit**

```bash
git add site/feedback/form.html
git commit -m "feat(feedback): form page HTML/CSS shell — track selection, prompts, progress bar"
```

---

## Task 3: Form Logic — Track Selection + Prompt Flow

**Files:**
- Modify: `site/feedback/form.html` (add JS)

- [ ] **Step 1: Implement track selection and prompt expansion logic**

Add JavaScript to `site/feedback/form.html` that handles:

1. **Track selection**: clicking a track card shows the prompt preview (all prompts collapsed with their question text visible). A "Inizia" button appears.
2. **Start flow**: clicking "Inizia" expands the first prompt, hides track selection, shows progress bar.
3. **Prompt navigation**: "Avanti" button on each prompt collapses it (with checkmark) and expands the next one. On last prompt, button says "Rivedi" instead.
4. **Review screen**: all prompts shown expanded with their transcript text areas. "Invia" button at bottom.
5. **Thank you**: after submit, replace everything with confirmation message.

State management — a simple object:

```javascript
var state = {
  track: null,           // 'veloce' or 'approfondito'
  sessionId: crypto.randomUUID(),
  currentIndex: 0,
  responses: [],         // { promptIndex, promptText, response, inputMethod, submitted }
  phase: 'select'        // 'select' | 'preview' | 'recording' | 'review' | 'done'
};
```

Navigation logic:
- `showPreview(track)` — show all prompts collapsed, "Inizia" button
- `startFlow()` — set phase to 'recording', expand first prompt
- `advancePrompt()` — save current response, collapse, expand next (or go to review)
- `showReview()` — expand all prompts with editable textareas
- `submitAll()` — mark all as final, POST any edited ones, show thank you

- [ ] **Step 2: Test the navigation flow manually**

Open the page, select "Approfondito", verify:
- Preview shows all 4 prompts collapsed
- "Inizia" expands the first
- Typing in the textarea and clicking "Avanti" collapses it with a checkmark
- Progress bar updates (1/4 → 2/4 → etc.)
- After last prompt, "Rivedi" shows review screen
- "Invia" shows thank you

- [ ] **Step 3: Commit**

```bash
git add site/feedback/form.html
git commit -m "feat(feedback): form navigation logic — track selection, prompt flow, review screen"
```

---

## Task 4: Web Speech API — Voice Recording + Transcription

**Files:**
- Modify: `site/feedback/form.html` (add speech recognition JS)

- [ ] **Step 1: Add Web Speech API recording logic**

Add to the form page JS:

1. **Feature detection**: check if `window.SpeechRecognition || window.webkitSpeechRecognition` exists. If not, hide all record buttons and show a note: "Il tuo browser non supporta la registrazione vocale, ma puoi scrivere il tuo feedback qui sotto."

2. **Recording state per prompt**: each prompt card gets a record button that toggles recording on/off.

3. **Speech recognition config**:

```javascript
function createRecognition() {
  var SR = window.SpeechRecognition || window.webkitSpeechRecognition;
  var recognition = new SR();
  recognition.lang = 'it-IT';
  recognition.continuous = true;
  recognition.interimResults = true;
  return recognition;
}
```

4. **Recording flow**:
   - Click "Registra" → button changes to "Stop" with pulsing red indicator
   - `recognition.onresult` appends transcript to the textarea in real-time
   - Interim results shown in lighter color, final results in normal color
   - Click "Stop" → `recognition.stop()`, button reverts, textarea is editable
   - User can re-record (clears and starts fresh) or edit manually

5. **Track input method**: set `inputMethod` to `'voice'` if recording was used, `'text'` if only typed.

- [ ] **Step 2: Test in Chrome**

Open in Chrome, click record:
- Microphone permission prompt appears
- Speaking in Italian produces real-time transcript
- Stopping preserves the transcript
- Editing the transcript after recording works
- Re-recording clears and starts fresh

- [ ] **Step 3: Test fallback in Firefox/Safari**

Open in Firefox:
- Record button is hidden
- Text area is visible and functional
- Fallback note is displayed

- [ ] **Step 4: Commit**

```bash
git add site/feedback/form.html
git commit -m "feat(feedback): Web Speech API voice recording with Italian transcription"
```

---

## Task 5: Google Sheets Submission

**Files:**
- Modify: `site/feedback/form.html` (add POST logic)

**Prerequisite:** Gate 0 must be complete. You need the Google Apps Script URL.

- [ ] **Step 1: Add submission logic**

Add the POST function and wire it into the prompt flow:

```javascript
var SCRIPT_URL = 'PASTE_YOUR_APPS_SCRIPT_URL_HERE';

function submitResponse(promptIndex, promptText, response, inputMethod, isFinal) {
  var payload = {
    session_id: state.sessionId,
    timestamp: new Date().toISOString(),
    track: state.track,
    prompt_index: promptIndex + 1,
    prompt_text: promptText,
    response: response,
    is_final: isFinal,
    input_method: inputMethod,
    user_agent: navigator.userAgent
  };

  fetch(SCRIPT_URL, {
    method: 'POST',
    mode: 'no-cors',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(payload)
  }).catch(function(err) {
    console.error('Submit failed:', err);
  });
}
```

Wire into existing flow:
- `advancePrompt()` calls `submitResponse(...)` with `isFinal: false` as each prompt is completed
- `submitAll()` in review screen re-submits any edited responses with `isFinal: true`, then shows thank you

Note on `mode: 'no-cors'`: Google Apps Script doesn't return proper CORS headers for POST. The request still goes through, but we can't read the response. Since we're fire-and-forget (no error handling needed for beta), this is fine.

- [ ] **Step 2: Test end-to-end**

1. Open the form, select Veloce, record a short message
2. Click submit
3. Check the Google Sheet — a new row should appear
4. Test Approfondito — complete 2 of 4 prompts, close the tab
5. Check the Sheet — 2 rows should be present with `is_final: false`
6. Complete a full Approfondito flow with review — 4 rows with last batch `is_final: true`

- [ ] **Step 3: Commit**

```bash
git add site/feedback/form.html
git commit -m "feat(feedback): Google Sheets submission — per-prompt POST with partial save"
```

---

## Task 6: Prompt Integration

**Files:**
- Modify: `prompt.md` (add feedback section at the end)

- [ ] **Step 1: Add feedback surfacing rules to prompt.md**

Add before the closing of the prompt file:

```markdown
<feedback>
When the session ends naturally — the user wraps up, completes a module, or says
something positive about the experience — mention the feedback page once:

"Se vuoi, puoi condividere come è andata qui: https://giannimassi.github.io/career-compass/feedback/
Mi aiuta tantissimo a migliorare questo strumento per chi si trova nella tua situazione."

Rules:
- Surface ONCE per session, at the end — never mid-flow
- NEVER surface when the user is frustrated, demoralized, or showing confidence-collapse signals (modal uncertainty + search stoppage)
- NEVER surface when you've just triggered a Small Win task or setback inoculation
- If the user didn't find the session useful, do not ask for feedback — they already told you
- Keep it casual, one sentence + link, no pressure
</feedback>
```

- [ ] **Step 2: Verify the section is well-placed**

Read the full `prompt.md` and confirm the `<feedback>` block doesn't interfere with existing sections (crisis protocol, persona, modules, etc.). It should be near the end, after the session flow rules.

- [ ] **Step 3: Commit**

```bash
git add prompt.md
git commit -m "feat(prompt): surface feedback link at natural session-end moments"
```

---

## Task 7: Landing Page Link + Deployment

**Files:**
- Modify: `site/index.html` (add feedback link)
- Modify: `.github/workflows/pages.yml` (ensure feedback/ is included)

- [ ] **Step 1: Add a subtle feedback link to the landing page**

Add a small link in the footer area of `site/index.html`. Match existing style. Something like a text link "Hai provato Career Compass? Lascia un feedback →" that links to `./feedback/`.

This should be subtle — the landing page is for new users, not testers. The link is there so testers who land on the main page can find the feedback form.

- [ ] **Step 2: Verify the GitHub Actions workflow deploys feedback/**

Read `.github/workflows/pages.yml`. The workflow uploads `./site` as the artifact. Since `site/feedback/` is inside `site/`, it will be included automatically. No workflow changes needed.

- [ ] **Step 3: Test locally**

Open `site/index.html` — verify the feedback link appears and points to `./feedback/`.
Open `site/feedback/index.html` — verify CTA links to `./form.html`.
Open `site/feedback/form.html` — verify full flow works.

- [ ] **Step 4: Commit**

```bash
git add site/index.html
git commit -m "feat(site): add subtle feedback link to landing page footer"
```

---

## Task 8: Add .gitignore for brainstorm artifacts

**Files:**
- Modify or create: `.gitignore`

- [ ] **Step 1: Add .superpowers/ to .gitignore**

```
.superpowers/
```

- [ ] **Step 2: Commit**

```bash
git add .gitignore
git commit -m "chore: gitignore brainstorm artifacts"
```

---

## Task 9: Push and Verify Deployment

- [ ] **Step 1: Push to main**

```bash
git push origin main
```

- [ ] **Step 2: Verify GitHub Pages deployment**

Check that the Actions workflow completes successfully. Verify:
- `https://giannimassi.github.io/career-compass/feedback/` loads the welcome page
- `https://giannimassi.github.io/career-compass/feedback/form.html` loads the form
- Full flow works end-to-end: select track → record → submit → check Google Sheet

---

## Execution Notes

- **Task 5 depends on Gate 0** — the Google Apps Script URL must be provided before wiring submission
- Tasks 1-4 can proceed independently of the Google Sheet setup
- Task 6 (prompt changes) is independent of all other tasks
- Task 7 depends on Tasks 1-2 existing
- Task 8 can be done anytime
- Suggested execution order: Gate 0 (human) → Tasks 1, 6, 8 (parallel) → Task 2 → Task 3 → Task 4 → Task 5 → Task 7 → Task 9
