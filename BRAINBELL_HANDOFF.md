# Brain Bell: handoff brief for Claude

Give this file to Claude at the start of a new session. It holds everything decided so far, so work can start without repeating the research.

**Prepared:** 26 November 2026
**Owner:** the Brain Bell developer (a senior frontend engineer; Angular, TypeScript, Tailwind, tested pure functions)
**Product:** https://brainbell.app

---

## 0. Instructions for Claude

1. Read this whole file before doing anything.
2. Ask the owner to attach the **Brain Bell repository** if it isn't in the session yet. Nothing in Part A can be done without it.
3. Check every fact below against the live code and site before acting on it. This file is a snapshot from 26 November 2026.
4. Start with **Part A** (SEO fixes). Then do **Part B** (Brain Bell Check), week by week.
5. Keep the product's principles (section 1.3). Never add accounts, ads, trackers or a paid AI API without asking.
6. Work on a branch, open a draft pull request, and write tests for all logic.

---

## 1. What Brain Bell is

### 1.1 The product
A **free live quiz game**. A host opens a quiz and shows a QR code and a 6-character join code on a big screen. Players scan it and play on their own phones. There's no sign-up, no app to install, no ads and no per-player fee.

Features:
- ready-made quizzes for Playgroup through Grade 8, plus quizzes for parties and work events;
- school quizzes that **generate new questions every game**;
- a quiz maker with **12 question types**;
- teams, and a "Bell Boss" the whole room plays against together;
- **daily puzzles** for grade bands 2–3, 4–5 and 6–7, with a streak counter and offline support;
- printable quizzes;
- results downloadable as a spreadsheet;
- WCAG 2.2 AA accessibility.

What people use most: **printable quizzes** and **live quizzes**.

### 1.2 How it's built (seen from the live site)
- **Angular 22.2.5**, prerendered: static HTML for each route, then a JavaScript bundle (`main-*.js`) takes over.
- Pages are served with trailing slashes (`/free-quiz-maker/`). Addresses without the slash get a **308 redirect** to the slashed version.
- `www.brainbell.app` redirects (301) to `https://brainbell.app/`.
- Quizzes are stored **in the browser**. A running quiz exists only in server memory and disappears when it ends.
- Real 404 pages work correctly.
- The sitemap is at `/sitemap.xml` with 74 URLs, and **has no `<lastmod>` dates**. `robots.txt` allows everything.

### 1.3 Principles to keep
- Free, no accounts, no ads.
- No student personal data on the server: fun names, seat numbers, or roster codes whose names stay in the browser.
- Works on any device in the browser, works offline where possible, any language, including right-to-left scripts.
- Accessible by default.
- Small, tested, pure logic at the core.

---

## 2. Current Search Console picture (export dated 26 Nov 2026, last 28 days)

| Period | Clicks | Impressions | Typical position |
|---|---|---|---|
| 28 Oct – 9 Nov | 4 | 35 | barely visible |
| 10 – 24 Nov | 81 | 4,154 | ~60, improving to ~30–35 |
| 18 – 24 Nov | 68 | 2,583 | ~35 |

- The site **grew** during Google's November 2026 core update (10–24 Nov). It wasn't hurt.
- **"quiz maker printable" and "printable quiz maker" = 31 of 85 clicks (36%)**, with click rates of 30–47%.
- Broad searches like "quiz maker" and "online quiz maker" sit at position ~55–60 with almost no clicks.
- Grade pages rank for **"… with answers"** searches:
  - "grade 3 math quiz with answers": position ~8;
  - "english quiz for class 4 with answers": position ~9;
  - `/quiz-library/grade-3-maths/`: 109 impressions at position ~14, **0 clicks**.
- Team-building and trivia pages get 700+ impressions at positions 39–80 and almost no clicks, so they're low priority.
- Countries:

| Country | Position | Click rate |
|---|---|---|
| Pakistan | ~14 | 7.9% |
| Bangladesh | ~22 | 6.4% |
| India | ~19 | 1.4% |
| United States | ~52 | 0.85% (most impressions) |

- Devices: **82% of impressions are on desktop.** Teachers find Brain Bell on laptops.
- **Next step:** ask the owner for a fresh export around **8 December 2026** and compare it with this one.

---

## 3. Google rules to follow (as of Nov 2026)

- **AI content isn't penalised for being AI.** Unhelpful or wrong content is.
- Google's AI content guidance was **updated on 1 Oct 2026**. A person must **fact-check all AI-generated content before publishing**, including titles, meta descriptions, structured data and alt text.
- **Scaled content abuse** is the main risk: many templated pages made mainly to rank. The August and September 2026 spam updates enforced it.
- A **back button hijacking** spam rule has been enforced since **15 June 2026**. Never add browser history entries that stop the Back button from leaving the site, including through third-party scripts.
- Search Console gained a **generative AI performance report** in June 2026 (AI Overviews, AI Mode, Discover). Watch it.
- **There's no "90-day sandbox."** Keep developing. Publish new pages gradually (2–4 truly different pages a week), not in big batches.
- Use the sitemap as the main way to get pages indexed. Use "Request indexing" only for one or two key pages at a time.

Re-check these on Google Search Central before relying on them. They were collected from news coverage on 26 Nov 2026.

---

## PART A: SEO fixes for brainbell.app (do these first)

### A1. Trailing-slash internal links (highest priority)
- **Problem:** 2,804 internal links use addresses without the trailing slash, for example `href="/free-quiz-maker"`. Each one redirects (308), so Google records both addresses and splits clicks between them:

| Page | Clicks | Impressions |
|---|---|---|
| `/free-quiz-maker/` | 26 | 718 |
| `/free-quiz-maker` | 24 | 1,097 |

  The same happens on many grade pages.
- **Fix:** every internal link to a content page should use the trailing-slash form, matching the canonical tags and the sitemap.
  - Check how links are made (`routerLink` or plain `href`) and fix them in one central place if possible.
  - Angular's router normally drops trailing slashes, so test that navigation still works in the browser and that the prerendered HTML contains `href="/…/"`.
- **Check:** `grep -o 'href="/[^"#?]*"'` over the built HTML. Every content link should end with `/`.

### A2. App pages that look like empty copies of the homepage
- `/host`, `/join` and `/quizzes` return status 200 with the **homepage's title** and almost no text until JavaScript runs. They have no canonical and no robots tag.
- **Fix:** add `<meta name="robots" content="noindex">` and a unique `<title>` to each, and keep them out of the sitemap.

### A3. Sitemap `<lastmod>`
- Add a correct `<lastmod>` to every sitemap URL, using the date the page's content last changed, not the build date.

### A4. About page
- Add an About page saying who builds Brain Bell and why, and link it from the footer.

### A5. Structured data gaps
- `/daily/`, `/teachers/`, `/privacy/` and `/accessibility/` have no JSON-LD. Add suitable types, for example `WebPage` or `AboutPage`. Keep it accurate.

### A6. Thin templated pages (do gradually, about 10 pages a week)
- **59 pages** have only about 120–300 words of readable text: `/quiz-library/*` (53) and `/bell-ringers/*` (6).
- For each page add:
  - **5–8 real sample questions with answers** as visible text;
  - a short "how teachers use this" with a real classroom tip;
  - what the quiz covers.
- Use regional wording: "class 4" (Pakistan, India), "year 4" (UK), "grade 4" (US).
- **A person must check every question and answer**, especially any written with AI.
- **Order:** start with pages that already get impressions:
  1. grade-3-maths
  2. grade-2-maths
  3. grade-5-maths
  4. grade-4-english
  5. grade-5 hub
  6. quiz-library hub

### A7. New page: printable quiz maker
- Create `/printable-quiz-maker/` (check first that no current page already targets this), aimed at "printable quiz maker" and "quiz maker printable". Include:
  - a sample printed quiz and answer key;
  - steps for printing;
  - tips for teachers;
  - a link to the quiz maker.
- Later, this page links to Brain Bell Check ("print it, then mark it with your phone").

---

## PART B: Brain Bell Check (the next product)

### B1. What it is
A new **section of brainbell.app**, not a separate app and not a native phone app. The teacher:
1. **Prints** a Brain Bell quiz with an answer sheet;
2. **scans the stack of papers with their phone's camera** in the browser;
3. sees **which questions and topics the class got wrong**;
4. taps **"Make tomorrow's review"**, which opens a Brain Bell quiz on only the missed topics;
5. sends **parents a short message** in their language.

Problem it solves: teachers with 40–120 students mark paper by hand and never turn the results into the next lesson. Existing photo-grading apps grade one paper at a time, mostly in English, on subscriptions, and stop at the score.

### B2. Firm decisions
- Lives **inside brainbell.app**. Same codebase, domain, privacy policy and brand.
- **No AI service and no paid API.** Everything runs in the teacher's phone browser for free.
- **No server needed for version 1.** Results are stored in **IndexedDB** on the device.
- First test market: **Pakistan and India** (best Search Console positions; large classes; paper-based).

### B3. Routes

| Route | Purpose | Indexing |
|---|---|---|
| `/check/` | Public landing page: what it is, sample sheet, how it works | **index** (launch in January) |
| `/check/scan` | Camera scanner | noindex |
| `/check/results` | Class grid, gaps, review button | noindex |
| `/check/parents` | Parent messages | noindex |
| `/p/<code>` | 2-minute practice for a parent or student | noindex |

Load the scanner code **only on `/check/*`** (lazy-loaded route) so the quiz pages stay fast.

### B4. The answer sheet (printable upgrade)
- **Four solid black corner squares** (about 36 px on an A4 page at 600×848) let the scanner straighten the photo.
- **A QR code** holding `quizId`, `version` and `page`. **Never put answers in the QR code**, or students could scan it.
- **Roll-number bubbles** (tens and units, 0–9), so one master copy can be photocopied for the whole class.
- Three answer formats:
  - `bubble` (A–D or True/False): read by image maths;
  - `digits` (one digit per box): read by a tiny on-device model (version 2);
  - `teacher` (words or sentences): the teacher marks them with a tap.
- **One layout JSON file is the source of truth for both the printable and the scanner:**

```json
{
  "quizId": "frac-g5-8q", "version": 3, "paper": "A4",
  "markers": [[42,42],[558,42],[42,806],[558,806]],
  "rollNumber": { "rows": 2, "x0": 178, "y0": 196, "dx": 38, "dy": 40, "r": 11 },
  "questions": [
    { "n": 1, "type": "bubble", "options": 4, "x0": 372, "y": 322, "dx": 52, "r": 12, "skill": "add-fractions" },
    { "n": 4, "type": "digits", "boxes": 2, "x": 372, "y": 496, "skill": "fraction-of-number" },
    { "n": 7, "type": "teacher", "x": 56, "y": 670, "w": 300, "h": 30, "skill": "word-problems" }
  ]
}
```

### B5. Scanning pipeline (proven in a prototype: 24 of 24 sheets read correctly, about 8 ms per sheet)

Run it in a **Web Worker**, on frames about 520–1000 px wide, about 5 frames a second:

1. **Camera:** `getUserMedia({ video: { facingMode: "environment" } })`, which needs HTTPS. Draw frames to an `OffscreenCanvas`, then convert to grayscale.
2. **Adaptive threshold** using an integral image: a pixel is dark if `pixel * count < windowSum * 0.78`, with a window half-size of about 24 px. This handles shadows.
3. **Find dark blobs** with a flood fill. Keep solid, square-ish blobs:
   - area roughly 300–3200 px at 520 px width;
   - aspect ratio 0.55–1.8;
   - fill ratio above 0.55;
   - not touching the image edge.
4. **Pick the four corners** among those candidates: top-left = smallest x+y, top-right = largest x−y, bottom-left = largest y−x, bottom-right = largest x+y. Fail if they aren't four distinct blobs, and show "Move closer or into more light".
5. **Perspective transform:** solve the 4-point equation (8×8 Gaussian elimination) from the sheet's marker centres to the photo's blob centres.
6. **Score each bubble.** Map about 29 sample points inside 0.55 × radius and take the average darkness. Compare it with the paper brightness: the 60th-percentile value of 12 points on a ring at 1.75 × radius. `score = 1 − inside / paper`.
7. **Decide each row:**
   - best score below 0.30 → **blank**;
   - second score at least 0.30 and above 0.65 × best → **double**;
   - otherwise → the best bubble.
   The roll number is decided the same way, for the tens row and the units row.
8. **Auto-capture** when 3 frames in a row agree. Then beep, vibrate where supported, show "✓ Roll 27 · 6/8", and save to IndexedDB.

Libraries:
- **Prefer plain TypeScript** for the steps above, about 300 lines, as in the prototype. Full OpenCV.js (8–10 MB) is too heavy for low-cost Android phones.
- **QR codes:** use the browser's `BarcodeDetector` where it exists (Chrome on Android), otherwise `zxing-wasm` or `jsQR` (needed on iPhone Safari).
- **Digits (version 2):** a tiny model of a few hundred KB, trained on MNIST/EMNIST and run with TensorFlow.js or ONNX Runtime Web. Below about 90% confidence, send the answer to teacher review. Urdu and Arabic-Indic digits need extra training data later.

### B6. After scanning
- **Results grid:** students × questions, each cell correct, wrong or blank/double. Clicking a question shows who missed it and the most common wrong answer, with advice:
  - 40% or more missed → re-teach the whole class;
  - 15% or more → help a small group;
  - otherwise → move on.
- **Teacher review screen:** every student's cropped answer to one question on one screen, with the correct answer on top. The teacher taps the wrong ones. No AI.
- **"Make tomorrow's review":** count misses per `skill` tag and open Brain Bell with something like `/host?skills=fraction-of-number&grade=5&count=5`. Use Brain Bell's **existing question generator** (number templates per skill), not AI.
- **Parent messages:**
  - fixed translated templates with blanks for name, score and skill (English, Urdu, then more);
  - sent with **`https://wa.me/<number>?text=<encoded>`** click-to-chat links, which are free and need no WhatsApp API;
  - alternatives: the Web Share API and printed slips;
  - parent numbers stay in the browser.
- **Export:** CSV, plus Excel via SheetJS.

### B7. Laptop-to-phone handoff (needed: 82% of searches are on desktop)
- Quizzes live in the browser of the device that made them, often a laptop.
- On the laptop, show "Scan with your phone to mark papers". That QR code opens `brainbell.app/check/scan#<encrypted quiz payload>`.
- The part after `#` is never sent to a server.

### B8. Phone browsers to support

| | Low-cost Android (Chrome) | iPhone (Safari) |
|---|---|---|
| Speed | Slower: use small frames and a Web Worker | Usually fast |
| QR reading | Built-in `BarcodeDetector` | Needs `zxing-wasm` or `jsQR` |
| Vibration | Yes | No: use a beep |
| Offline | Cache `/check` with a service worker | Same |

### B9. Privacy
- Student codes, not names. Names stay in the teacher's browser.
- Photos are processed in memory and never uploaded.
- No accounts.

### B10. SEO for Check
- Only `/check/` is indexable. Every app screen is noindex.
- Keep normal navigation; never trap the Back button.
- Launch the landing page in **January 2027, at the start of term**, linked from `/printable-quiz-maker/`. Request indexing for that one or two URLs only.

### B11. Build plan

| Week | Deliverable |
|---|---|
| 1 | Layout JSON; upgraded printable answer sheet; scanner reading **uploaded photos** (no live camera yet); unit tests; a folder of ~100 real phone photos (tilted, shadowed, pencil, photocopied) as test cases |
| 2 | Live camera in `/check/scan`, Web Worker, auto-capture, beep; results grid; CSV export |
| 3 | Skill tags and the "Make the review" link; parent messages (`wa.me`, slips); offline caching; laptop-to-phone QR handoff |
| 4 | Teacher review screen; digit boxes with the tiny model; test with 5–10 real teachers on noindex pages |

**Definition of done for the scanner:** reads at least 98 of 100 real test photos correctly, and never silently gives a wrong score (uncertain sheets must be flagged).

---

## 4. Reference material

Interactive pages published as private claude.ai artifacts. The owner can open them; reopen them from the claude.ai artifacts gallery if the links don't work in a new session.
- **Brain Bell Check idea brief:** https://claude.ai/artifact/B8c4zJrxwp1KfTUCuAAniR
- **Working scanner prototype:** https://claude.ai/artifact/297EuQEbKXhLYCtHmAHNLG. This holds the full reference implementation of section B5 in plain JavaScript. Read it with the Artifact tool's `read` action and port the `scan()`, `homog()`, `bil()` and `decide()` functions to TypeScript.
- **Sub binder idea (parked):** https://claude.ai/artifact/JiqoUoWS48TxbjGZCbUd4z

Other ideas considered and parked:
- **Sub binder:** a "sub plans" tool. US searches are about 9,900 a month with low SEO difficulty. It could later reuse Brain Bell quizzes as emergency lessons.
- **Live polls and Q&A:** a Mentimeter-style tool.
- **Hifz tracker:** progress tracking for madrasa teachers.

Other sites by the same owner: dailytimeflow.com (date and time tools) and tool-calculator.com. getspecwrite.com wasn't reachable on 26 Nov 2026, and specwrite.com showed a parked "/lander" page. None of these earn meaningful value yet, which is why the focus is Brain Bell.

---

## 5. Open questions to ask the owner
1. Brand: "Brain Bell Check" inside brainbell.app (recommended), or a different name?
2. Can they recruit 5–10 teachers who already print Brain Bell quizzes, ideally in Pakistan, to test week 4?
3. Which parent-message languages come after English and Urdu?
4. Do schools in the test group use Western digits (123) or Urdu/Arabic-Indic digits (۱۲۳)?
5. A new Search Console export around 8 December 2026, to compare with section 2.

## 6. First message to send Claude
> "Read BRAINBELL_HANDOFF.md. I've attached the Brain Bell repository. Start with Part A, fixes A1 and A2, on a new branch, with tests, and open a draft PR."
