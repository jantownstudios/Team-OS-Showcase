English translation of [the German original](HANDOFF-design-52.md). The original is the working document; this translation was made afterwards for readers.

# Handoff: Design 52 · Showcase for job applications: README, diagram, screenshots — role „Design“

As of: 2026-11-10 · Issued by: Master · Report to: `docs/ABSCHLUSS-design.md` (overwrite; predecessor Design 51, in Git) · Workplace: **`rollen\design`** (run `git merge main` first) · Mode: **Implementation (Canvas: no)** · Round: **medium** · Commit nothing.

## What this is about
Jan is applying for jobs and wants to show recruiters (HR, not developers) what he has built. For this there will be a **separate public repo** `jantownstudios/Team-OS-Showcase`. It contains only a README with screenshots and a diagram. The code repo `TeamOS-App` stays private. You deliver the content, the Master reviews it, creates the repo and publishes it with Jan's go.

## Jan's decisions (2026-11-10)
- **Separate showcase repo**, `TeamOS-App` stays private.
- **German.**
- **No code** in the showcase, only README, screenshots, diagram, numbers.
- **Name:** `Team-OS-Showcase`.

## Read first (read only, write nothing there)
- `F:\Team-OS\START.md`, `F:\Team-OS\kern\*.md` — how the system works (Master, roles, handoff, completion report, review stages).
- `F:\Team-OS\projects\teamos-app\PATCHNOTES.md` — what the app can do.
- `F:\Team-OS\projects\teamos-app\analyse\A-aufbau.md`, `B-abgleich.md`, `C-potenzial.md`, `Z-zahlen.md`, `G-begriffe.md` — finished description and classification.
- `docs/design/SPEZIFIKATION.md` and `tokens.json` — look and colours.

## Task
1. **`schaufenster/README.md`** in your workplace, in German, readable for HR (no technical terms without explanation, short paragraphs, tables instead of running text where possible). Content:
   - **What it is:** an operating system for an AI team. For each project a **Master** plans with Jan, **roles** (Bau, Design, Test, Gegenprobe, Recherche) work in the background from a written brief, everything runs in a Windows app of its own (Rust).
   - **Jan's role, honestly:** Jan designed the system (rules, roles, workflows, review stages), makes the decisions and signs off every delivery. **The code is written by the AI roles.** Do not embellish, do not play it down.
   - **How the work is done:** plan → brief (handoff) → role delivers with completion report → Master checks itself → Gegenprobe (red team) tries to get past the safeguards → Jan signs off on the screenshot → release. Why: report ≠ review ≠ sign-off.
   - **What the app can do:** a selection, short (start and supervise roles, approvals with one click, dashboard for switching off tools, own browser with block list, messages between Masters, access from phone and laptop). **No ways past safeguards, no vulnerabilities, no red-team findings in detail.**
   - **Numbers** (measured by the Master, take them as they are): start 2026-10-27 · **29 versions** from 0.1.0 (2026-10-28) to 0.2.14 (2026-11-10) · 488 commits · rounds: Bau 73, Design 51, Gegenprobe 29, Recherche 13, Test 17 · 708 automated tests in the last full run · about 29 projects in the app. More numbers only from `Z-zahlen.md`, with date.
   - **Screenshots** (point 3) and **diagram** (point 2) embedded, relative paths.
2. **Diagram** `schaufenster/bilder/aufbau.png` (and `.svg`, if your tool can do it): Jan · Master · roles · app (approvals, watchdog, dashboard) · Claude. In the Souli look (tokens). Readable on GitHub light and dark (own background in the image). Tool under `docs/design/werkzeug/design52.py`.
3. **Screenshots of the app**, 3 to 5, under `schaufenster/bilder/`. Take them from the check modes that work with **test data** (`cargo run -j 4 --bin pruefung -- <modus>`, e.g. `bau69`, `bau62`, `bau55`; Rust environment as in `docs/ROLLEN.md`; debug build only) or from `docs/design/entwuerfe/`. **Look at** every screenshot **yourself** and check: no real data of Jan's, no addresses, no domain, no IP, no names except project names from test data. Do not take a screenshot you cannot check with certainty.
4. **Checklist** in the completion report: for each file in the folder `schaufenster/` one sentence on why it may be public. In addition, a search over the whole folder for `[entfernt]`, `[entfernt]`, `[entfernt]`, `[entfernt]`, `@`, `C:\`, `F:\`, `Users` with the result.
5. **Open questions** to the Master, brief, each with a recommendation (e.g. contact line, author's name, licence note).

## Limits
- Only `schaufenster/**` in your workplace, `docs/design/werkzeug/design52.py`, output of the check modes in `.pruefung/` and your completion report. No app code, no SPEZ, do not change tokens.
- No personal data of Jan's (name, e-mail, place of residence, health). No home-network details, no server providers, no domain.
- Do not copy other people's texts or tables; only summarise the core and the analysis.
- No visible window, do not create a repo, publish nothing, no `gh`.

## Files
- **May:** `schaufenster/**`, `docs/design/werkzeug/design52.py`, `.pruefung/**`, `docs/ABSCHLUSS-design.md`
- **May not:** everything else
