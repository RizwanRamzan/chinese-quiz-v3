# ChinesePod Quiz — Developer Guidelines for Rizwan

## Overview

You are building standalone quiz pages for ChinesePod lessons.
Each quiz lives at its own URL and functions as an independent module — it is **not** embedded inside the lesson page.

**URL pattern:** `chinesepod.com/{lessonNumber}/quiz`
Example: `chinesepod.com/4628/quiz`

The lesson number is your primary identifier for everything.

---

## Tech Stack — What You Can and Cannot Use

| Allowed | Not Allowed |
|---------|-------------|
| Bootstrap 5.3.3 (CDN link is fine) | jQuery or any other JS library |
| Vanilla JavaScript only | Any npm package (ask Nadir first if you really need one) |
| Web Speech API, Web Audio API, Canvas API (all browser-native) | External CSS frameworks other than Bootstrap |
| Built-in Node.js modules only (if any server-side logic needed) | Any npm module without prior approval |

**CSS:** Write all your styles inside a `<style>` tag in the same file. No external `.css` files.

**JavaScript:** All JS inside `<script>` tags in the same file. Vanilla only.

---

## Brand Guidelines

Download ChinesePod's official brand kit here:
**http://chinesepod.com/corporate-identity**

Follow the color palette, typography, and logo usage rules from that document. Your quiz must look like it belongs on ChinesePod.com.

---

## No-Scroll Rule

**Everything must fit on screen without vertical scrolling at 1280×720 resolution.**

Design for 1280×720 as your minimum viewport. If your quiz has 8 questions, only show one question at a time — never stack all questions on one page. The intro screen, quiz screen, and results screen should each fill the viewport cleanly.

---

## How to Communicate with the Backend

All backend communication uses `fetch()` with `POST` to `window.location.href`.
Pass an `action` field in the JSON body to tell the server what you need.

### Action 1 — Save Quiz Result

Call this when the user finishes the quiz (on the results screen).

```javascript
fetch(window.location.href, {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    action: 'updateQuizProgress',
    lessonNumber: '4628',   // use the actual lesson number from the URL
    userScore: 7,            // questions answered correctly
    totalScore: 8            // total questions
    // add any other fields you need — tell Nadir what you are sending
  })
})
.then(res => res.json())
.then(data => {
  // tell Nadir exactly what you expect in `data` here
  // example: data.success, data.attemptNumber, data.bestScore — you decide
});
```

> Before Nadir builds this handler, let him know the full list of fields you plan to send, and the exact JSON response your app needs back (e.g. `{ success: true, attemptNumber: 2, bestScore: 8 }`). He will build the server to match whatever you specify.

---

### Action 2 — Get User Details

Call this on page load if you need to personalise the quiz (e.g. show the user's name).

```javascript
fetch(window.location.href, {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    action: 'getUserDetails',
    fields: ['username', 'email']  // list only the fields you actually need
  })
})
.then(res => res.json())
.then(data => {
  // data will contain the fields you requested
  // example: data.username, data.email
});
```

> Let Nadir know which fields you actually need (username, email, level, avatar, etc.) and he will include them in the response.

---

### Action 3 — Get Quiz History

Call this if you want to show the user their previous attempts (e.g. "Your best score: 7/8, 3 attempts").

```javascript
fetch(window.location.href, {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    action: 'getQuizHistory',
    lessonNumber: '4628'
  })
})
.then(res => res.json())
.then(data => {
  // tell Nadir what format you expect
  // example: data.attempts (array), data.bestScore, data.totalAttempts
});
```

> Let Nadir know what shape you need this response in (e.g. `data.attempts` as an array, `data.bestScore`, `data.totalAttempts`) and he will build it to match.

---

## Before You Start Building — Share This with Nadir

Send Nadir the following so he can build the backend handlers:

1. **updateQuizProgress** — full list of fields you will POST (score, time taken, per-question results, etc.) and the exact JSON response your app expects back
2. **getUserDetails** — which user fields your quiz needs (username, email, level, avatar, etc.)
3. **getQuizHistory** — what JSON shape you expect in the response
4. **Any npm module** you think you need — confirm with Nadir before using it

---

## Deliverables

3 quiz files, one per level, identified by lesson number:

| Level | Example Lesson | File to deliver |
|-------|---------------|-----------------|
| Newbie | (your choice) | `quiz-[lessonNumber]-newbie.html` |
| Pre-Intermediate | (your choice) | `quiz-[lessonNumber]-preint.html` |
| Upper Intermediate | (your choice) | `quiz-[lessonNumber]-upperint.html` |

Each file must be fully self-contained (CSS in `<style>`, JS in `<script>`, no external files other than Bootstrap CDN and ChinesePod brand fonts if applicable). OR you can discuss this with MG how many levels you will be working on initially

---

## Summary Checklist

- [ ] Bootstrap 5.3.3 via CDN only
- [ ] Vanilla JS only — no jQuery, no libraries
- [ ] All CSS inside `<style>` tag
- [ ] All JS inside `<script>` tag
- [ ] Fits 1280×720 with zero vertical scroll
- [ ] Follows ChinesePod brand guidelines (`chinesepod.com/corporate-identity`)
- [ ] Uses `fetch POST window.location.href` with `action` parameter for all backend calls
- [ ] Told Nadir your expected request/response structure for each action
- [ ] 3 quiz files delivered (Newbie, Pre-Int, Upper-Int)
