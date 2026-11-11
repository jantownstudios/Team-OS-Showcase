English translation of [the German original](ABSCHLUSS-bau-73.md). The original is the working document; this translation was made afterwards for readers.

# Completion report Build 73 · Kill switch „Einlese-Sperre“ (read-in lock) in the dashboard (0.2.14)

Start 2026-11-10 11:45 · End 2026-11-10 12:07 · Round: **small** · Workplace `rollen\bau` (branch `rolle/bau`, `main` merged at `5bdbb50`) · uncommitted · debug build only (`-j 4`, `--offline`) · version not touched · no new dependency · **0 `claude` calls** · no visible window (screenshots headless) · `F:\Team-OS`, `docs/design/**` not touched, test data only in `.pruefung/` · F: 25.7 GB free at the end.

## Please know first

**All five acceptance points have a test or a screenshot, everything is green.** I checked with the app's real pipe and the real hook code. No real `claude` ran.

**Screenshots for Jan** (rendered headless, mock assistant):
- `F:\TeamOS-App\rollen\bau\.pruefung\bau73\dashboard-1920-an.png` and `…\dashboard-1920-aus.png`
- `F:\TeamOS-App\rollen\bau\.pruefung\bau73\dashboard-1280-an.png` and `…\dashboard-1280-aus.png`

**The place is on the right** in a block of its own, „MASTER · 1“, between EXTERN (external) and SPERRLISTE (block list) (interpretation 1). That is the only piece that smells of design. Please decide on the screenshot.

## What changed

| File | Change |
|---|---|
| `crates/teamos/src/werkzeuge.rs` | `EINLESE_SPERRE` (key in `dashboard.json`), field `einlese_aus`, `einlese_sperre()`, `einlese_sperre_kippen()`; reading and writing; test `einlese_sperre_notschalter` |
| `crates/teamos/src/dashboard.rs` | block „MASTER“ with the row „Einlese-Sperre“; the global switch of a row (word, NeoMaster, tooltip, focus) pulled out of `zeile()` as `schalter()`, so that both use the same one; `Tat::EinleseSperre` |
| `crates/teamos/src/oberflaeche.rs` | `werkzeuge_pflegen` and `master_anlegen` pass the switch to the connector of every Master page (2 lines) |
| `crates/assistent/src/lib.rs` | `Assistent::einlese_sperre(an)`, default: do nothing |
| `crates/assistent/src/claude/mod.rs` | `Innen.einlese_aus` (default `false` = lock on); answer to `stand` and `einlesen_stop`; `einlese_sperre()` on `ClaudeAssistent`; test `notschalter_einlese_sperre` |
| `crates/assistent/src/claude/einlesen.rs` | `Einlesen::stopp_ohne_sperre()`: through, count of rejections to 0 (7 lines) |
| `crates/teamos/src/bin/pruefung_bau73/`, `pruefung.rs` | check tool `… -- bau73` (screenshots, click, file, restart) |

## How it works

1. **Storage.** `dashboard.json` in the app's data folder gets `"einlese_sperre": true|false`.
   - **Only a stored `false` switches it off.** If the file or the key is missing, the file is broken or unreadable, or it holds anything other than a boolean (`"aus"`, `0`, `null`, object, list): the lock is **on**.
   - An unknown value stays in the file when the app writes for another reason. Only Jan's click replaces it.
   - It is **not** an entry in the `KATALOG` and **not** a `Werkzeug` in the plug. `global_kippen`, `projekt_setzen` and `wie_global` do not know the ID and do nothing. There are no exceptions per project. A `werkzeuge.json` with `einlese_sperre` is not read.
2. **Jan's click.** The diamond in the dashboard calls `einlese_sperre_kippen()`. That flips the value, writes `dashboard.json` safely and counts up `stand`. There is no confirmation, because there are no exceptions that would be deleted. That is the same rule as for a global switch without exceptions (§15.5).
3. **Path to the connector.** `werkzeuge_pflegen` sees the new `stand` and calls `assistent.einlese_sperre(wert)` for every open Master page. A new Master page gets the value in `master_anlegen` before its first start. The connector keeps it only in memory (`Innen.einlese_aus`). Until the first call, „on“ applies there.
4. **Effect.**
   - Hook (`stand`): For a Master, `einlesen` is then `[]`. For a role it stays `null` as before.
   - Stop hook (`einlesen_stop`): The answer is „through“, and the sequence of rejections starts anew.
   - The stream keeps counting the files read, unchanged. If Jan switches it back on, the app answers with the real state.
   - Hook, Stop hook and the settings of `claude` are unchanged. Because both ask the app on every call, the switch takes effect **immediately**, also in running sessions.
   - If the app cannot be reached or the answer is not genuine, the lock stays on in the hook as before. The switch only opens through a genuine, signed answer.

## Interpretations + reason (for decision)

1. **Place: on the right, a block of its own „MASTER · 1“** between EXTERN and SPERRLISTE.
   - Label row and spacing (40 above the label, 16 to the block) are as for SPERRLISTE, the row is like a tool row without „Ausnahmen“ (exceptions) and „Zuletzt“ (last used).
   - Right label text: „gilt für alle Master-Sitzungen, Rollen nie“ (applies to all Master sessions, never to roles).
   - **Reason:** The left already has eight rows, that column is clearly longer than the right one. At 1920 × 1080 the page is 1470 high; on the right the content ends after two block-list entries at about 700. At the bottom in INTERN (internal) it would have made the difference even bigger and would only be visible after scrolling.
   - It does not fit under EXTERN, because it is not from outside.
   - **The only new thing about it is the label „MASTER“.** There was no existing pattern for a single row without a group of its own. If Design wants another place or name, it is a move of a few lines.
2. **Icon: the one of the wish log (three lines).** It stands for „read text“. There is no icon of its own, and inventing one belongs to Design. There is a precedent for an icon used twice: „Meldungen aufs Handy“ (notifications to the phone) and „Handy-App“ (phone app) also share one.
3. **Description:** „Master lesen nach jedem Start zuerst ihre Pflichtdateien ganz.“ (Masters first read their required files in full after every start.), word for word the example from the handoff, one line.
4. **Tooltip on hover** over the diamond: It is the existing tooltip „Für alle n Projekte“ (For all n projects). It fits, because the switch applies to all projects.
5. **Stop hook on „off“ resets the count to 0.** Otherwise a rejection before switching off would have brought the exit forward after switching back on. For this there is a small method in `einlesen.rs`. The file is not named explicitly, but belongs to the „answer to `einlesen_stop`“.
6. **No app message to the Master** when switching (unlike with plugs). The handoff does not ask for one. The Master notices it because the hook no longer blocks.
7. **Number in the sidebar** („Dashboard 8“) and „INTERN · 8“ stay at 8, because the read-in lock is not a tool of the AI.

## Check (my own, not Jan's sign-off)

**Tests** (logs in `.pruefung/bau/`):
- `cargo test -j 4 -p assistent --offline -- --skip zweites_claude --skip wurzel_chrome_und_node`: **110 passed, 0 failed**, 1 of them new (`tests-bau73-assistent.txt`).
- `cargo test -j 4 -p teamos --lib --offline -- dashboard werkzeuge master …`: **141 passed, 0 failed**, 1 of them new (`tests-bau73-teamos.txt`).
- `cargo clippy -p assistent -p teamos --tests --bins`: no warning in the touched files.
- No full run.

| Acceptance | Result | Evidence |
|---|---|---|
| 1 Off in a running session, on again | Master with 9 missing required files. Before: `Write` = `deny`. **Off:** `stand` → `einlesen: []`; `Write` goes through without asking; `Bash`/`rolle_starten` without a read-in message; `git push` still asks (`ask`, previous rules); Stop hook five times „through“, no line „Einlesen unvollständig“ (read-in incomplete); a `Read` of START.md is still counted. **On again:** `stand` names the real 8 missing ones, `Write` = `deny` without START.md in the list; Stop hook again three times `block`, then through | `notschalter_einlese_sperre` (real pipe, real hook code) |
| 2 Missing, broken, unknown | Lock on when: file missing; `{kaputt`; empty; `[]`; key missing; `"aus"`, `0`, `null`, object, list; value only under `global`; folder instead of file (unreadable). An unknown value stays when writing | `einlese_sperre_notschalter` |
| 3 Restart | Click → `false` in the file; read again: off; flipped again, read again: on. In the check tool via the real click on the diamond, per size | `einlese_sperre_notschalter`, `… -- bau73` (`.pruefung/bau73/bericht.txt`, 0 errors) |
| 4 No other way | **Pipe:** Requests with the kinds `einlese_sperre`, `einlesen_an`, `einlesen_aus`, `einlese_sperre_setzen`, `stecker` and fields `an`/`einlese_sperre`/`wert` change it in neither direction (the app files unknown kinds as an open request and does not answer, so asked with a deadline). **`werkzeuge.json`:** `{"ausnahmen": {"einlese_sperre": true}, "einlese_sperre": true}` in the project changes nothing. **Other write paths:** `global_kippen`, `projekt_setzen`, `wie_global` with the ID, other switches, block list, „zuletzt“ (last used) do not change it; the plug stays the same. **Web app:** In the code there is no way to it. `einlese_sperre_kippen` is called only by `dashboard.rs` (click). `crate::mobil*` has no access to `Werkzeuge`, and `dashboard.json` is on the list of forbidden files there (`mobil/tests_bau61.rs`) | `notschalter_einlese_sperre`, `einlese_sperre_notschalter`, `grep` |
| 5 Screenshot of both positions | 1920 × 1080 and 1280 × 720, each on and off, paths above | `cargo run -j 4 --bin teamos-pruefung -- bau73` |

## Open points

1. **Old check step Build 40:** `pruefung_bau40` checks „all places `raute-*` belong to the `KATALOG`“. With `raute-einlese_sperre` this one point now fails. I did not adjust it, because it is an old report. If needed, exempt `einlese_sperre` there.
2. **Proof with a real `claude`** (Master with Jan): Start a Master, do not let it read its required files, switch off in the dashboard. The next `Write` must go through, and the answer must not be held back. Then switch on: blocked again.
3. **Laptop and phone** do not have the switch (as the handoff says). The web app does not show it.
4. **Icon** (interpretation 2) and **place/label** (interpretation 1) for decision by Design or the Master.
