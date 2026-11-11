**English** · [Deutsch (original)](fall-abnahme.de.md)

*Translated from German. Quotes are given in the German original, followed by a translation.*

# Case: a delivery I rejected

**Project:** a desktop player of mine for a music streaming service · **Date:** 2026-10-14 · **Rounds:** Design 7 and
Build 6

Two roles reported the main window as checked. I looked at it in the real window and did not sign it off. Instead I
handed back a list of eight points. Four of them were bugs, and the roles had explicitly reported two of those as
checked. A new rule for every role in this project came out of it.

The sources are private: the project's code repo (completion reports, handoffs and rules, each with its commit) and the
Master's decision log and changelog in Team-OS. Quotes are word for word. I do not name the project or the service here.
One point concerns signing in to the service; I only count it.

## 1 · What the roles reported as checked

Design 7 built the main window (start page, search, the large "Now playing" view, a bar at the bottom), Build 6 the
part behind it. Both rounds were merged on 2026-10-14 at 03:29.

| Round | What the completion report says was checked | What it names itself as not checked |
|---|---|---|
| Design 7 (commit `60f1519`) | „**63 von 63 Prüfungen grün**“ (*63 of 63 checks green*); under "How it works": „Ein Klick auf Cover und Text der Bar öffnet „Jetzt läuft““ (*a click on the cover and text of the bar opens "Now playing"*) | „im echten Tauri/WebView2-Fenster“ (*in the real Tauri/WebView2 window*), „mit echtem Herzstück, echten Daten und echten Bildern“ (*with the real core, real data and real images*) |
| Build 6 (commit `0c6e3aa`) | „**177 Tests grün**“ (*177 tests green*); for playback in a test run: „200, 4 s später neuer Song läuft (3,5 s)“ (*200, 4 s later a new song is playing*) | search in the real window: „konnte ich ohne sichtbares Fenster nicht messen“ (*I could not measure without a visible window*) |

The "Herzstück" (core) is the part of the player that runs in the background.

## 2 · What I found in the real window

My list is in the Master's decision log under "2026-10-14 · Abnahme Hauptfenster, Liste geschlossen (Jan)" (*sign-off
main window, list closed*). The quotes are my sentences as the Master wrote them into the new briefs (handoffs Design 8
and Build 7, commit `8f7cd48`). The view itself was fine; the brief to Design 8 says: „Die Ansicht selbst passt („echte
Vorschläge, Theme, Souli, Bar")“ (*the view itself is right: real suggestions, theme, Souli, bar*).

| # | What I found | Kind |
|---|---|---|
| 1 | „Oben ist eine Leiste“ (*there is a bar at the top*): the Windows title bar. I wanted a frameless window with its own buttons. | wish |
| 2 | „Ich kann die Themes nicht ändern“ (*I can't change the themes*): I wanted to switch the colour scheme inside the app. | wish |
| 3 | „Unten in der Taskleiste ist nicht Souli“ (*Souli is not in the taskbar*): a placeholder was still there. | still missing |
| 4 | concerns signing in to the service (only counted) | still missing |
| 5 | „Wenn ich auf einen Song drücke, lädt er unten, startet aber nicht.“ (*when I press a song, it loads at the bottom but doesn't start*) | **bug**, reported as checked |
| 6 | „Wenn ich suche, erscheint direkt ‚hat zu lange gebraucht'.“ (*when I search, "took too long" appears straight away*) | **bug**, reported as not measured |
| 7 | At start the window said „Herzstück nicht erreichbar“ (*core not reachable*), although the player was only still loading. | **bug** |
| 8 | „Wo ist die große Ansicht, wenn ich unten auf den Balken klicke?“ (*where is the large view when I click the bar at the bottom?*) | **bug**, reported as checked |

The "Kind" column is my own classification for this page, not the source's. Point 7 is on my list; in the brief to
Design 8 the Master lists it as „Befund des Masters“ (*the Master's finding*).

## 3 · Why the checks missed it

The Master measured the causes on the running player before writing the new briefs (handoffs Design 8 and Build 7,
commit `8f7cd48`).

**Point 5, song does not start.** The song was loaded but paused. The cause is the browser's autoplay block: playback
only starts after an input the browser considers human. The brief to Build 7 says:

> „Deine Prüfungen in Bau 3–6 starteten Songs mit `userGesture: true` — das hat es verdeckt.“
>
> *Translation:* "Your checks in Build 3–6 started songs with `userGesture: true`; that hid it."

`userGesture: true` marks an input in a test as if it came from a human. The player itself has no such mark. So the
block never showed in the checks, and showed at once in the real window.

**Point 8, clicking the bar opens nothing.** The brief to Design 8:

> „An der Klickstelle liegt die Cover-Ebene (`.ebene`) **über** dem Knopf `#bar-oeffnen`; ein echter Mausklick trifft das Bild, nichts öffnet sich. Deine Prüfung klickte im Code.“
>
> *Translation:* "At the click point the cover layer (`.ebene`) lies **above** the button `#bar-oeffnen`; a real mouse click hits the image, nothing opens. Your check clicked in code."

Design 7's check called the button directly in the program. A real mouse click hits whatever is on top, here the image.

**Point 6, search.** Measured: „504 nach 0,0 s“ (*504 after 0.0 s*). The error did not come after waiting but at once.
In the real window the search could not run at all, and the player reported that as "too long". Build 6 had named
exactly this case as not measured (section 1 above).

**Point 7, start message.** The lists were loaded before the player had reported its first status, so the page showed
the "not reachable" error text.

**Points 1 to 4** are not missed bugs but things I wanted different in the real window, or that were still missing.

## 4 · The new rule

It has been in the project's `docs/ROLLEN.md`, the rules for all roles, since commit `8f7cd48` (2026-10-14, 03:40), and
is unchanged there today. The merge was at 03:29; the rule came eleven minutes later, in the same commit as the two new
briefs. Original wording:

> - **Echte Eingaben, keine Abkürzungen** (Präzedenz 2026-10-14: zwei Fehler kamen erst bei Jan raus). Klicks und
>   Tasten in Prüfungen **wie ein Mensch**: DevTools `Input.dispatchMouseEvent`/`Input.dispatchKeyEvent` **an der Stelle,
>   wo man klickt** — nicht `element.click()` im Code (sonst merkt man nicht, wenn etwas darüber liegt). **Kein
>   `userGesture: true`** und keine anderen Markierungen „das war ein Mensch" beim Prüfen von Abspielen/Steuern — das
>   Produkt hat sie auch nicht (sonst bleibt z. B. Chromes Autoplay-Sperre unsichtbar). Was nur mit sichtbarem,
>   fokussiertem Fenster geht, im Abschluss ausdrücklich als „nicht gemessen" nennen.

*Translation:*

> - **Real inputs, no shortcuts** (precedent 2026-10-14: two bugs only came out with Jan). Clicks and keys in checks
>   **like a human**: DevTools `Input.dispatchMouseEvent`/`Input.dispatchKeyEvent` **at the spot where you click**, not
>   `element.click()` in code (otherwise you don't notice when something lies on top). **No `userGesture: true`** and no
>   other "this was a human" marks when checking playback/controls; the product doesn't have them either (otherwise
>   e.g. Chrome's autoplay block stays invisible). Whatever only works with a visible, focused window must be named
>   explicitly as "not measured" in the completion report.

Also in the decision log: „**Jan: erst alles sammeln, dann eine Runde.**“ (*Jan: collect everything first, then one
round.*) All eight points went into one round per role (Design 8, Build 7) instead of fixing each point on its own.

## 5 · What changed afterwards

- **Design 8** (commit `5685920`) first checked the bar against the old state: „**Vorher-Probe** mit der Seite aus Git
  (HEAD): Der Klick aufs Cover öffnet nicht, die Prüfung findet den Fehler also wirklich.“ (*before-check with the page
  from Git (HEAD): the click on the cover does not open, so the check really finds the bug*). Then with a real mouse
  click at four spots: „Cover, Titel, Künstler und Leerraum → „Jetzt läuft““ (*cover, title, artist and empty space →
  "Now playing"*). The completion report says: „Kein `element.click()`, kein `userGesture`.“ And it has its own list
  „**Nicht gemessen** (geht nur mit dem echten, sichtbaren Fenster)“ (*not measured, only possible with the real,
  visible window*).
- **Build 7** (commit `4c50d6c`) checked playback without any mark and wrote itself that this was not enough: the test
  run without a window „beweist Punkt 1 nicht“ (*does not prove point 1*; its point 1 is my point 5), „Beweis nur im
  echten Fenster.“ (*proof only in the real window*).
- **My next sign-off** the same day (decision log, "Hauptfenster abgenommen bis auf Kleinigkeiten", *main window
  signed off except for small things*): „**Musik startet** (Autoplay-Fix bestätigt).“ (*music starts, autoplay fix
  confirmed*). Search still did not work in the real window, for a second reason; an addendum to Build 8 fixed that.
- **In the evening** (decision log, "MVP-Abschluss"): „Suche, Lautstärke, Songtext, Neustart nach X funktionieren“
  (*search, volume, lyrics, restart after X work*). On 2026-10-15 I signed off the first finished version.

The pattern behind this applies across Team-OS: a report is not a review, and a review is not a sign-off. Here only the
sign-off showed what the checks could not see, and since then the rule has changed how checks are done.
