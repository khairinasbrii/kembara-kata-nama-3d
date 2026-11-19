# AGENTS.md — Kembara Kata Nama 3D

## Project Overview
Web-based 3D educational game for Year 1 KSSR Bahasa Melayu pupils (nouns / "kata nama"). Players explore a virtual school in first person, click objects, and answer noun quizzes. Deployment target is GitHub Pages (static hosting only — no server).

Design reference: `software design document.txt` (single-file design doc, in Malay — the authoritative spec for features, zones, scoring, and milestones). Read it before changing gameplay.

## Tech Stack
- **Three.js** — 3D rendering
- **Vite** — build tool
- **Firebase / Firestore** — leaderboard, auth (client-side only; config in `src/firebase/firebaseConfig.js`)
- **Browser Speech Synthesis API** — all audio narration in Bahasa Melayu (no audio files; do not add a TTS backend)

## Planned Structure (repo is currently design-stage)
```
src/
├── assets/   (models/, sounds/, icons/)
├── scenes/   (schoolScene.js)
├── systems/  (quizManager.js, scoreManager.js, audioManager.js, leaderboard.js)
├── data/     (nouns.json)
├── firebase/ (firebaseConfig.js)
├── ui/       (hud.js, menu.js)
├── main.js
└── index.html
```
Follow this layout when creating files — new systems go in `src/systems/`, UI in `src/ui/`, static word data in `src/data/`.

## Commands
Not yet initialized. Once Vite is set up:
- `npm install`
- `npm run dev` — dev server
- `npm run build` — production build (verify GitHub Pages base path if deployed under a subpath)

## Architecture Rules
- Game systems (quiz, score, audio, leaderboard) are separate modules wired together in `main.js`; keep them decoupled from Three.js scene code.
- Firestore score document shape: `{ playerName, score, stars, completionTime, createdAt }` — do not change without updating the leaderboard UI.
- Leaderboard supports daily / weekly / all-time (top 10) views.

## Domain & Content Rules
- All UI text and narration is **Bahasa Melayu** for 7-year-olds.
- School zones and their nouns (Zon A Pintu Masuk, B Kelas, C Kantin, D Taman, E Padang) are defined in the design doc; quiz distractors include non-nouns (adjectives, verbs) intentionally.
- Gamification: +1 star per correct answer, +10 coins for special objects, badges at 10 missions / >90% score / all animal nouns found.

## UX / Accessibility Constraints (mandatory)
- Large text, high contrast, large icons, cheerful colors, minimal reading.
- Every instruction must also be spoken aloud (Speech Synthesis).
- Must work on both desktop and mobile browsers.
