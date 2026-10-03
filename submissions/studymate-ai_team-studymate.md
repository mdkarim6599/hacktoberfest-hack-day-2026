# StudyMate AI

Snap your notes → get an explanation, a quiz and revision keywords. Powered by **Gemma 4**
(multimodal) via the **Gemini API**.

## Team / attendee

- Team name (if applicable): Team StudyMate
- Members and GitHub usernames: Md karim (@mdkarim6599), Abbani Vaishnavi (@abbanivaishnavi), minnatullah (@Minnatullah1)
- Profile links (optional): https://github.com/mdkarim6599 · https://github.com/abbanivaishnavi · https://github.com/Minnatullah1

## Challenge

Select the challenge you are entering:

- [ ] Best Open-Source AI Project
- [x] Best Use of Gemma 4
- [ ] Build on elah

## Project links

- Public GitHub repository: https://github.com/mdkarim6599/gemma4-hackday-2026
- Open-source license (link to the license file): https://github.com/mdkarim6599/gemma4-hackday-2026/blob/main/LICENSE (MIT)

## Problem and solution

**Who is this for?** Students revising from **photos of handwritten notes, textbook pages and
diagrams**.

**Problem:** turning that raw material into something you can actually *study from* — a simple
explanation, practice questions, and the keywords that matter — is slow manual work. Generic
chatbots return an unstructured wall of text with nothing to practise against.

**Main input → output workflow:**

```
INPUT:  a photo of notes/diagram  (or a typed topic)
        + difficulty (Beginner / Intermediate / Exam-ready)
        + language (English / Hinglish)
                    │
                    ▼
        Gemma 4 (multimodal) reads the material
                    │
                    ▼
OUTPUT: a structured "study pack"
        🧠 explanation  ·  💡 real-life analogy  ·  📌 key points
        🔑 exam keywords  ·  ❓ multiple-choice quiz with live scoring + explanations
```

## Approach and technologies

- **UI:** Python + **Streamlit** (`app.py`) — file upload, difficulty/language controls, and
  Explanation / Quiz / Revision tabs with live quiz scoring.
- **AI step:** `gemma_client.generate_study_pack()` calls **Gemma 4** through the **Gemini API**
  with `google-genai`, asking for **JSON only** against a fixed schema.
- **Validation:** the JSON is parsed and validated into a `StudyPack` object (MCQ count, 4 options
  each, `answer_index` clamped to range). Malformed output is retried once; if the API fails, a
  cached pack is shown so a live demo never breaks. The UI only renders validated structured data.
- **Why these choices:** Streamlit + a single structured model call keeps the whole workflow
  deliverable and demoable quickly, and forces the model's output to be structured rather than a
  chat reply.

## Challenge evidence

### Best Use of Gemma 4

- **Gemma 4 model identifier and Gemini API integration:** `gemma-4-26b-a4b-it` (Gemma 4,
  open-weights, served through the Gemini API) — called against
  `generativelanguage.googleapis.com` with the official `google-genai` SDK; the model id is
  configurable via `GEMMA_MODEL` and the key is read from `.env` (`GEMINI_API_KEY`).
- **Code link showing the integration:**
  [`gemma_client.py` → `generate_study_pack()`](https://github.com/mdkarim6599/gemma4-hackday-2026/blob/main/gemma_client.py)
  (builds the prompt + JSON schema, sends the image and/or topic, validates the response).
- **Input and useful output; multimodal value:** the student's real input is a **photo of notes**,
  not typed text. Gemma 4 reads the image **directly** (no separate OCR step), then produces the
  structured study pack. Multimodality adds value because the source material is visual — including
  handwriting, layout and small diagrams — and the same model converts that understanding into
  study material. Output is immediately useful: a readable explanation, exam keywords, and a quiz
  the student can attempt and score.

## Current status

- **What works:** image input and topic input; difficulty and language (English/Hinglish) controls;
  explanation + analogy + key points + exam keywords; MCQ generation with live scoring and answer
  explanations; JSON validation with retry; cached fallback pack for demo safety.
- **Known limitations / incomplete features:** no accounts or saved history; the quiz is
  self-scored on one page; study-plan and summariser modes from the original idea are not built yet.
- **What you would improve next:** save study packs per topic, spaced-repetition reminders, and a
  summariser mode for long pasted notes.

## Submission checklist

- [x] Project repository is public and links work.
- [x] Required challenge evidence is included.
- [x] Project uses an open-source license where required by the challenge (MIT).
- [x] Work and reused materials are represented honestly.
- [x] No API keys, tokens, passwords, or private data are included (`.env` is gitignored).
- [x] I followed the organizers' build window and submission instructions.
