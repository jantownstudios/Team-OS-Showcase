English translation of [the German original](ABSCHLUSS-design-52.md). The original is the working document; this translation was made afterwards for readers.

# Completion report: Design 52 · Showcase for job applications: README, diagram, screenshots

Role: Design · Round: medium · Mode: Implementation (Canvas: no) · **Start 2026-11-10 15:11 · End 2026-11-10 15:24** (computer clock)
Working copy `F:\TeamOS-App\rollen\design` (branch `rolle/design`, `main` merged in at `4ee9f3c`). Nothing committed, no repo created, no `gh`. Handoff `docs/HANDOFF-design-52.md`.

**Result:** The content for `Team-OS-Showcase` is in `rollen\design\schaufenster\`: a README in German, a diagram (PNG + SVG) and five screenshots of the app from check modes with test data. The search for the eight words finds nothing. Four questions are open, above all the first-person form (interpretation 1).

## What changed
| File | What |
|---|---|
| `schaufenster/README.md` | new: intro, At a glance, What it is (terms), My role (honestly), How the work is done (8 steps + why), What the app can do (7 features), screenshots, numbers, technology |
| `schaufenster/bilder/aufbau.png` (1600 × 1000), `aufbau.svg` | new: diagram human · Master · roles · app with approvals, watchdog, dashboard · Claude |
| `schaufenster/bilder/01-master-seite.png` | Master page with the card „Wie soll Bau 33 aufhören?“ (How should Bau 33 stop?) (from `bau33`, `zustand1-1920`, scaled down to 1600) |
| `schaufenster/bilder/02-dashboard.png` | Dashboard (from `bau73`, `dashboard-1920-an`, cut off at the bottom, 1600 wide) |
| `schaufenster/bilder/03-team-center.png` | Team Center in the laptop view (from `bau69`, `tc-1366`, cut off at the bottom) |
| `schaufenster/bilder/04-roadmap.png` | Roadmap in the laptop view (from `bau69`, `rm-1366`, cut off at the bottom) |
| `schaufenster/bilder/05-freigabe-handy.png` | Approval card in the chat on the phone (from `bau56`, `offen`, 786 × 1704) |
| `docs/design/werkzeug/design52.py` | new: paints the diagram, crops the screenshots, searches for the words, writes `.pruefung/design52-pruefung.json` |

## How it works
- **Tool:** `python docs/design/werkzeug/design52.py` from the repo folder. The check modes must have run before
  (`cargo run -j 4 --bin teamos-pruefung -- bau33 | bau73 | bau69 | bau56`, debug build). The list `AUSWAHL` names for each
  screenshot the source, the crop with its reason, and the width. Saved with PIL without text metadata.
- **Diagram:** The SVG is built from the tokens: `farbe.grund` with sun and moon light, glass (milk 0.05,
  edge 0.14, light edge 0.16), `radius.karte` 18. The role colours follow `farbe.rolle.reihenfolge`, i.e. the place in
  `ROLLEN.md`. The Master's edge runs in the gradient sun → moon like „Brainstorm“. Chrome without a window makes the PNG at 1×
  with Manrope from `assets/schriften`. The background is in the image, so it looks the same on GitHub light and dark.
  Font sizes ≈ 1.5 × the app's size, because GitHub shows the image at about half its width.
- **Search:** All files under `schaufenster/` as UTF-8 and UTF-16. For PNG, only those chunks count in which text
  can appear (`tEXt`, `zTXt`, `iTXt`, `eXIf`). In the pixels and in the header „@“ occurs by chance (width 1600 = hex
  `06 40`, and `0x40` is „@“). That was the first false alarm, see Check.
- **Same twice:** Three runs in a row gave the same checksums for all seven files.

## Interpretations + reason (for decision)
1. **README and diagram in the first person, without the name „Jan“.** Reason: The repo is Jan's own showcase, so
   an applicant speaks for himself. That way no name is in the public text either („keine persönlichen Daten: Name“ (no personal data: name)). In the
   diagram the human is called „Ich · Entwickler“ (Me · Developer). If it should say „Jan“, that is 7 texts in `design52.py` and the README.
2. **Screenshot 1 shows stopping a role**, not the Master handover (`bau43`, also clean). Reason: It shows
   chat, role stars and a decision card, and with that the core idea „Mensch entscheidet“ (the human decides).
3. **Crops:** In the dashboard the Team-OS path (`F:\…`) was at the bottom left, in Team Center and Roadmap at the bottom left
   the device name with Jan's first name. Both are cut away, nothing was painted over.
4. **Roadmap from the 1366 version:** In the check mode it is scrolled to the middle (the top row is cut in half).
   The 1920 one shows „wartet auf Jan“ (waiting for Jan) and a focus frame on the back button. That is why I take the 1366 one.
5. **Approvals in the README:** „Hochladen“ (uploading) stands for `git push`, so that HR understands it. The Gegenprobe is only described as a role
   („versucht, die Sperren zu umgehen“ (tries to get past the safeguards)), without findings and without the limits of the safeguards.
6. **Numbers:** The Master's numbers are there unchanged. From `Z-zahlen.md` only three are taken, with „Stand 2026-10-31“ (as of 2026-10-31)
   (22 projects with repo, 532 handoffs, 351 decisions). The line count from `A-aufbau.md` is not taken, because
   the handoff only allows `Z`.
7. **Watchdog in the diagram:** It stands for watchdog and hook together („prüft jeden Werkzeug-Aufruf, meldet hängende
   Rollen“ (checks every tool call, reports hanging roles)). Two boxes would only have given HR one more technical term.

## Check (my own, not Jan's sign-off)
**Checklist per file:**
| File | Why it may be public |
|---|---|
| `README.md` | It summarises the core, patch notes and analyses in my own words, without copied tables. There is no name, no paths, addresses, domains, providers or home-network details and no safeguard findings. The numbers come from the Master or from `Z`, each with a date. |
| `bilder/aufbau.png`, `aufbau.svg` | They are drawn by me and contain only terms of the system. No name, no logo of a third-party program (Claude only as a word). |
| `bilder/01-master-seite.png` | Test project „probe“, roles Bau/Design/Test, limits „noch kein Stand“ (no reading yet). The screenshot has been looked at and contains no path and no name. |
| `bilder/02-dashboard.png` | The block list shows only examples from the check mode (`example.org/geheim`, `paypal.com`). The path at the bottom is cut off, the screenshot has been looked at. |
| `bilder/03-team-center.png` | Test projects kaderplaner, notizen, teamos-app, made-up inbox messages. The device name is cut off, the screenshot has been looked at. |
| `bilder/04-roadmap.png` | made-up plan rows from `bau69`. The device name is cut off, the screenshot has been looked at. |
| `bilder/05-freigabe-handy.png` | Test project teamos-app, made-up chat, command `cargo test … lauf_7`. The screenshot has been looked at. |

**Search** over the whole folder for `[entfernt]`, `[entfernt]`, `[entfernt]`, `[entfernt]`, `@`, `C:\`, `F:\`, `Users`
(UTF-8 and UTF-16; for PNG the text chunks, present are only `IHDR`, `IDAT`, `IEND`): **no hits**
(`.pruefung/design52-pruefung.json`). In the first run the search reported „@“ in all PNGs. That was due to pixels and width,
not text, and is fixed in the tool.

**Not taken:** `bau40/linie-*` shows a user path with a name, `bau45` a QR code with an address, `bau31` reads
real ROADMAPs, `bau55` has an empty chat.

**Check modes:** `bau73`, `bau69`, `bau43`, `bau33`: 0 errors. `bau55` reports 2 errors, `bau40` 2 and `bau56` a scene with
errors. All three are older modes against newer code; from them only `bau56/offen` is taken, and the screenshot has been
looked at and is complete. Not investigated, not my round.

**Not proven:** how GitHub actually shows the README (light/dark, width). The SVG without an embedded font shows
Segoe UI/Helvetica there instead of Manrope. That is why the README embeds the PNG.

## Open points (questions to the Master, each with a recommendation)
1. **First person or „Jan“?** Recommendation: keep the first person. That way the applicant speaks for himself, and no name is in the repo.
   The name is on the GitHub account and in the application anyway.
2. **Contact line?** Recommendation: none in the README. The application carries the contact. If Jan wants one, then only a link
   to his GitHub profile, no e-mail (the search for „@“ then stays clean).
3. **Licence note?** Recommendation: one line „Alle Rechte vorbehalten, Bilder und Text nur zur Ansicht“ (All rights reserved, images and text for viewing only) and no open
   licence, because it is not code for reuse. Manrope is only rendered in the PNG, not passed on. The SVG does not embed it
   (an OFL notice would otherwise be mandatory).
4. **Ship the SVG too?** Recommendation: yes, as an extra. The PNG is embedded because GitHub does not load a font in the SVG.
- In passing: `docs/design/werkzeug/__pycache__/` was already untracked in the working copy before this round (from earlier
  tool runs). I did not touch it.
