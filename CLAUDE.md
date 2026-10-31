# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

This directory is both an **Obsidian vault** (`.obsidian/`) and the source of a **marketing landing page** for the Hebrew University "AI Developers" course (קורס מפתחי AI). The site is in Hebrew, laid out right to left (`dir="rtl"`).

- `index.html`: the entire website in one file
- `פרויקטים/אתר קורס מפתחי AI/`: Obsidian project notes (overview, structure/design, open tasks with a dated log)

## Running

There is no build step, package manager, linter or test suite. Open `index.html` directly in a browser. On Windows: `Start-Process index.html`.

## Architecture of index.html

- All CSS sits in one `<style>` block and all JS in one IIFE `<script>` at the bottom. No inline handlers and no external JS. The only external resource is Google Fonts (Frank Ruhl Libre for display text, Heebo for body text, JetBrains Mono for code and labels).
- Design tokens are CSS custom properties on `:root` (`--ink`, `--stone`, `--gold`, `--space-*`, `--radius*`, `--shadow-*`). Use them rather than hard-coded values. Styles are mobile-first, with breakpoints at 640px and 1024px.
- Because the page is RTL, use logical properties (`inset-inline-*`, `border-inline-start`). LTR content such as the code terminal, tags, phone and email fields is explicitly set to `dir="ltr"` or `direction:ltr`.
- JS behaviours, each self-contained in the IIFE:
  - scroll-state header and mobile menu
  - `#neural` canvas network animation in the hero
  - typed terminal, driven by the `code` array of `[class, text]` tokens
  - curriculum accordion, using the `data-open` attribute plus `aria-expanded`
  - `.reveal` elements fading in via IntersectionObserver
  - lead form validation, using the `rules` object keyed by field id, with errors shown in `#<id>-err`
- Every animation respects `prefers-reduced-motion`. Keep that when adding motion.

## Content constraints

- Real course facts (dates, prices, hours, lecturers, schedule) are **placeholders in square brackets**, e.g. `[תאריך]`, `[שם המרצה]`. Never invent real values. Leave placeholders until the user supplies the facts.
- The curriculum modules and week ranges are a proposal, not the official syllabus.
- Form submission is simulated (`TODO` in the submit handler). No backend is connected yet.
- The official university logo and branding are deliberately not used until authorization is confirmed. Do not publish the site publicly (e.g. as a hosted artifact) under the university's name without the user's explicit go-ahead.

## Workflow (required for every change)

1. **Models:** plan with Opus 5.5 and carry out the implementation with Sonnet 5.5 (e.g. delegate the coding to a Sonnet subagent).
2. **Test:** once the code is written, test it, then run regression checks on existing behaviour. With no test suite, that means opening the page and checking the parts listed above: header and menu, canvas, terminal, accordion, reveal and form validation, at mobile and desktop widths.
3. **Security:** after testing, run the `security-review` skill.
4. **Commit:** finish every action that changes something with a git commit.
5. **Document in Obsidian:** at the end of the run, record every update in the notes in `פרויקטים/אתר קורס מפתחי AI/`. Tick tasks in `משימות פתוחות.md` and append a dated line to its `## יומן` log.
6. **Every new branch gets its own Obsidian note:** `ענף <branch>.md` in the same folder, covering its goal, what changed, tests and status. Link it from `סקירת הפרויקט.md`. The notes are tracked in git, so commit note updates on `master` and merge `master` into the other branches. Otherwise the note disappears when you switch branches.
