English translation of [the German original](HANDOFF-bau-73.md). The original is the working document; this translation was made afterwards for readers.

# Handoff: Build 73 · Kill switch „Einlese-Sperre“ (read-in lock) in the dashboard (0.2.14) — role „Bau“ (Build)

As of: 2026-11-10 · Issued by: Master · Report to: `docs/ABSCHLUSS-bau.md` (overwrite; predecessor Build 72, in Git) · Workplace: **`rollen\bau`** (run `git merge main` first) · Round: **small** · Commit nothing.

Read first: `docs/ABSCHLUSS-bau.md` (Build 72), `docs/HANDOFF-bau-71.md` (decisions), `crates/teamos/src/dashboard.rs` (header, row of a tool), `crates/teamos/src/werkzeuge.rs`, `crates/assistent/src/claude/mod.rs` (pipe kinds `stand` and `einlesen_stop`), `docs/design/SPEZIFIKATION.md` §15 (read only).

## Goal
Jan can switch off the read-in lock in the dashboard with one click if it blocks wrongly. The effect is immediate, also in running Master sessions, without a reinstall.

## Decisions by Jan (2026-11-10)
- „den Schalter in das Dashboard noch mit reinknallen“ („just slap the switch into the dashboard as well“), and that **in 0.2.14** (he has not updated yet). The version stays 0.2.14.

## Set by the Master
- **One global switch**, default **on**. No exceptions per project: those live in `.teamos\werkzeuge.json` in the code folder, and a Master must not be able to influence its lock through a file in its own area.
- **The direction on errors stays safe:** If the stored state is missing, broken or unreadable, the lock counts as **on**. Caution: the existing plugs know the opposite direction („unreadable = everything that can be switched off is off“). So do **not** build it as a `Werkzeug` in `Stecker.aus`, but so that an error never lifts the lock.
- **Only Jan's click in the app's interface changes it**, stored only in the app's data folder (like `dashboard.json`). No way via pipe, tool server, web app or a file in the project.
- **Effect in the app:** If the switch is off, the app answers `stand` with `einlesen: []` and `einlesen_stop` with „durch“ (through). Counting continues in the stream, so that the real state applies when it is switched back on. The hook and the Stop hook stay unchanged.
- **Look:** like an existing tool row in the dashboard (icon, name „Einlese-Sperre“, global switch, one line of description, e.g. „Master lesen nach jedem Start zuerst ihre Pflichtdateien ganz“ (Masters first read their required files in full after every start)), but without „Ausnahmen“ (exceptions) and „Zuletzt“ (last used). Choose the place so that both columns still end balanced. Do not invent a new look; if no existing pattern fits, write it in the report.
- Laptop and phone do not get the switch in this round.

## Limits
Only this switch. Roles stay without a read-in lock. No real `claude`, no visible window, no real data of Jan's, nothing in the Team-OS. Do not change the version. No new dependency. Tests matching the change (`assistent`, `teamos` with `dashboard`, `werkzeuge`, `master`), `-j 4`, exceptions from `docs/ROLLEN.md`; no full run. Do not empty the build folder.

## Files
- **May:** `crates/teamos/src/dashboard.rs`, `crates/teamos/src/dashboard/**`, `crates/teamos/src/werkzeuge.rs`, where needed the place in `crates/teamos/src/**` that passes the state to the connector, `crates/assistent/src/**` (only the answer to `stand`/`einlesen_stop` and the handover of the switch), tests, check tool, `docs/ABSCHLUSS-bau.md`, `.pruefung/bau/`
- **May not:** `docs/design/**`, `docs/ROLLEN.md`, `Cargo.toml`, `F:\Team-OS`, Git

## Check (acceptance)
Measured, each with a test:
1. Switch off: a running Master session with missing required files may immediately use all tools under the previous rules, the Stop hook lets it through. On again: the lock applies with the real read state.
2. Stored state missing, broken or with an unknown value: the lock is on.
3. After a restart of the app, the switch stays as Jan set it.
4. No way other than Jan's click changes it (pipe requests, `werkzeuge.json`, web app routes).
5. A screenshot of the dashboard with the new switch in both positions (taken invisibly, path in the report), so that Jan can look at it.
