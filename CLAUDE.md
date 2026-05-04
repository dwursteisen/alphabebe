# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

**AlphaBébé** — a French-language alphabet/word learning web app for toddlers (≈3–4 years old). The child taps letter cards to hear them spoken, taps the image to hear the full word, and parents can manage the word list (add a word + optional image, delete, reset to defaults).

## Architecture

The entire application is a **single self-contained file**: `index.html`. There is no build system, no package manager, no dependencies installed locally. All CSS, JS, and markup live in that one file.

- **No bundler / no transpiler.** Edits to `index.html` are immediately live on reload.
- **External assets:** only Google Fonts (Lexend) via CDN. No npm, no Yarn, no Vite.
- **Persistence:** user-customised word list is stored in `localStorage` under the key `alphabebe-words-v2`. When no entry exists, the list falls back to `DEFAULT_WORDS` (an in-file array of ~18 French words).
  - Backwards-compat: old entries that were plain strings are coerced to `{ text, image: null }` on load (see `loadWords`).
  - Custom images are read with `FileReader.readAsDataURL` and persisted as base64 strings inside the same JSON blob — keep this in mind, localStorage has a ~5 MB cap per origin so adding many large images will eventually fail silently.
- **Speech:** uses the browser's `SpeechSynthesis` API with `lang = 'fr-FR'`. The voices list is populated asynchronously, so the `voiceschanged` listener must remain in place — don't read voices only once at startup.
- **Render optimisation:** `renderWord()` only rebuilds the letter-card DOM when `currentWordIndex` changes (tracked via `renderedWordIndex`). Mutations that change letters in place (e.g. delete-current-word) must reset `renderedWordIndex = -1` to force a full rebuild — `removeWord` already does this; new mutators must too.

### State (module-level globals)

`words`, `currentWordIndex`, `currentLetterIndex`, `imageVisible`, `pendingImage`, `modalOpen`, `voices`, `renderedWordIndex`. There is no framework — these are plain `let` bindings driven by direct DOM updates.

### Keyboard shortcuts (desktop)

`Space` next · `←/→` move letter cursor · `↑/↓` prev/next word · `L` speak current letter · `M` speak word · `I` toggle image. The handler bails when a modal is open or focus is in an `<input>` — preserve that guard if you add new shortcuts.

## Running locally

No server needed. Open the file directly:

```bash
open /Users/david/workspace/perso/alphabebe/index.html
```

Speech synthesis works from `file://` in Chrome/Safari. There are no tests, no linter, no typecheck — verify changes by opening the page and exercising the flows manually (tap letters, tap image, add/remove a word, reload to confirm persistence).

## Design reference

`stitch_cran_d_apprentissage_principal/` holds a Google Stitch mock-up (`code.html`, `screen.png`) and a `DESIGN.md` describing the "Digital Playroom" design language (Lexend font, rounded `xl`-radius surfaces, no 1px borders — separate sections via background-shifts and spacing, no `#000` shadows). Consult it before introducing new visual patterns; the live `index.html` already follows these rules (Lexend, rounded cards, soft shadows tinted with the on-surface colour, generous whitespace).

## Conventions specific to this repo

- **Don't introduce a build step or framework** unless explicitly asked. The single-file constraint is intentional — it makes the app trivially deployable (drop `index.html` anywhere) and lets a non-technical parent fork it.
- **All UI strings are in French.** Keep new copy in French and informal-but-warm in tone (the audience is parents reading aloud to small children).
- **Letter colour map (`COLORS`) is keyed by accent-stripped lowercase letters.** Use `baseChar()` when looking up colours so accented characters (é, è, à…) resolve correctly.
- **`git` conventions for this repo are loose** (commits so far: `init`, `v1`). The Memo-Bank-wide rules in the user's global instructions (Angular Conventional Commits, GitLab MRs, etc.) do **not** apply here — this is a personal project under `~/workspace/perso/`.
