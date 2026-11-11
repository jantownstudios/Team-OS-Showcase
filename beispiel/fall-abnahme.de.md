[English](fall-abnahme.md) · **Deutsch (Original)**

# Fall: eine Lieferung, die ich zurückgewiesen habe

**Projekt:** ein Desktop-Player von mir für einen Musik-Streamingdienst · **Datum:** 2026-10-14 · **Runden:** Design 7
und Bau 6

Zwei Rollen meldeten das Hauptfenster als geprüft. Ich habe es am echten Fenster angesehen und nicht abgenommen,
sondern eine Liste mit acht Punkten zurückgegeben. Vier davon waren Fehler, zwei davon hatten die Rollen ausdrücklich
als geprüft gemeldet. Daraus entstand eine neue Regel für alle Rollen dieses Projekts.

Die Quellen sind privat: das Code-Repo des Projekts (Abschlüsse, Handoffs und Regeln, je mit Commit) und die
Entscheidungen und das Changelog des Masters im Team-OS. Zitate stehen wörtlich. Den Namen des Projekts und des Dienstes
nenne ich hier nicht. Ein Punkt betrifft die Anmeldung beim Dienst; den zähle ich nur.

## 1 · Was die Rollen als geprüft meldeten

Design 7 baute das Hauptfenster (Startseite, Suche, die große Ansicht „Jetzt läuft“, eine Bar unten), Bau 6 den Teil
dahinter. Beide Runden wurden am 2026-10-14 um 03:29 zusammengeführt.

| Runde | Was der Abschluss als geprüft meldet | Was er selbst als nicht geprüft nennt |
|---|---|---|
| Design 7 (Commit `60f1519`) | „**63 von 63 Prüfungen grün**“; unter „Wie es funktioniert“: „Ein Klick auf Cover und Text der Bar öffnet „Jetzt läuft““ | „im echten Tauri/WebView2-Fenster“, „mit echtem Herzstück, echten Daten und echten Bildern“ |
| Bau 6 (Commit `0c6e3aa`) | „**177 Tests grün**“; beim Abspielen in einem Testlauf: „200, 4 s später neuer Song läuft (3,5 s)“ | die Suche im echten Fenster: „konnte ich ohne sichtbares Fenster nicht messen“ |

Das „Herzstück“ ist der Teil des Players, der im Hintergrund läuft.

## 2 · Was ich am echten Fenster fand

Meine Liste steht im Entscheidungs-Log des Masters unter „2026-10-14 · Abnahme Hauptfenster, Liste geschlossen (Jan)“.
Die Zitate sind meine Sätze, wie der Master sie in die neuen Aufträge geschrieben hat (Handoffs Design 8 und Bau 7,
Commit `8f7cd48`). Die Ansicht selbst passte; im Auftrag an Design 8 steht: „Die Ansicht selbst passt („echte
Vorschläge, Theme, Souli, Bar")“.

| # | Was ich fand | Art |
|---|---|---|
| 1 | „Oben ist eine Leiste“: die Windows-Titelleiste. Ich wollte ein Fenster ohne Rahmen mit eigenen Knöpfen. | Wunsch |
| 2 | „Ich kann die Themes nicht ändern“: Ich wollte das Farbschema in der App umschalten. | Wunsch |
| 3 | „Unten in der Taskleiste ist nicht Souli“: dort stand noch ein Platzhalter. | fehlte noch |
| 4 | betrifft die Anmeldung beim Dienst (nur gezählt) | fehlte noch |
| 5 | „Wenn ich auf einen Song drücke, lädt er unten, startet aber nicht.“ | **Fehler**, als geprüft gemeldet |
| 6 | „Wenn ich suche, erscheint direkt ‚hat zu lange gebraucht'.“ | **Fehler**, als nicht gemessen gemeldet |
| 7 | Beim Start meldete das Fenster „Herzstück nicht erreichbar“, obwohl der Player nur noch lud. | **Fehler** |
| 8 | „Wo ist die große Ansicht, wenn ich unten auf den Balken klicke?“ | **Fehler**, als geprüft gemeldet |

Die Spalte „Art“ ist meine Einordnung für diese Seite, nicht die der Quelle. Punkt 7 steht in meiner Liste; im Auftrag
an Design 8 führt der Master ihn als „Befund des Masters“.

## 3 · Warum die Prüfung es übersah

Die Ursachen hat der Master am laufenden Player gemessen, bevor er die neuen Aufträge schrieb (Handoffs Design 8 und
Bau 7, Commit `8f7cd48`).

**Punkt 5, Song startet nicht.** Der Song war geladen, aber pausiert. Ursache ist die Autoplay-Sperre des Browsers:
Abspielen startet erst nach einer Eingabe, die der Browser für menschlich hält. Im Auftrag an Bau 7 steht dazu:

> „Deine Prüfungen in Bau 3–6 starteten Songs mit `userGesture: true` — das hat es verdeckt.“

`userGesture: true` markiert eine Eingabe im Test so, als käme sie von einem Menschen. Der Player selbst hat diese
Markierung nicht. In den Prüfungen war die Sperre darum nie zu sehen, am echten Fenster sofort.

**Punkt 8, Klick auf die Bar öffnet nichts.** Im Auftrag an Design 8:

> „An der Klickstelle liegt die Cover-Ebene (`.ebene`) **über** dem Knopf `#bar-oeffnen`; ein echter Mausklick trifft das Bild, nichts öffnet sich. Deine Prüfung klickte im Code.“

Die Prüfung von Design 7 rief den Knopf direkt im Programm auf. Ein echter Mausklick trifft aber, was oben liegt, hier
das Bild.

**Punkt 6, Suche.** Gemessen: „504 nach 0,0 s“. Die Fehlermeldung kam also nicht nach Warten, sondern sofort. Im
echten Fenster ließ sich die Suche gar nicht ausführen, und der Player meldete das als „zu lange“. Bau 6 hatte genau
diesen Fall als nicht gemessen genannt (oben, Abschnitt 1).

**Punkt 7, Startmeldung.** Die Listen wurden geladen, bevor der Player seinen ersten Stand gemeldet hatte, und die
Seite zeigte darum den Fehlertext für „nicht erreichbar“.

**Punkte 1 bis 4** sind keine übersehenen Fehler, sondern Dinge, die ich am echten Fenster anders haben wollte oder
die noch fehlten.

## 4 · Die neue Regel

Sie steht seit Commit `8f7cd48` (2026-10-14, 03:40) in `docs/ROLLEN.md` des Projekts, den Regeln für alle Rollen,
und dort bis heute unverändert. Zusammengeführt wurde um 03:29; die Regel kam elf Minuten später, im
selben Commit wie die zwei neuen Aufträge. Wortlaut:

> - **Echte Eingaben, keine Abkürzungen** (Präzedenz 2026-10-14: zwei Fehler kamen erst bei Jan raus). Klicks und
>   Tasten in Prüfungen **wie ein Mensch**: DevTools `Input.dispatchMouseEvent`/`Input.dispatchKeyEvent` **an der Stelle,
>   wo man klickt** — nicht `element.click()` im Code (sonst merkt man nicht, wenn etwas darüber liegt). **Kein
>   `userGesture: true`** und keine anderen Markierungen „das war ein Mensch" beim Prüfen von Abspielen/Steuern — das
>   Produkt hat sie auch nicht (sonst bleibt z. B. Chromes Autoplay-Sperre unsichtbar). Was nur mit sichtbarem,
>   fokussiertem Fenster geht, im Abschluss ausdrücklich als „nicht gemessen" nennen.

Dazu im Entscheidungs-Log: „**Jan: erst alles sammeln, dann eine Runde.**“ Alle acht Punkte gingen in eine Runde je
Rolle (Design 8, Bau 7), statt jeden Punkt einzeln nachzubessern.

## 5 · Was danach anders lief

- **Design 8** (Commit `5685920`) prüfte die Bar zuerst gegen den alten Stand: „**Vorher-Probe** mit der Seite aus Git
  (HEAD): Der Klick aufs Cover öffnet nicht, die Prüfung findet den Fehler also wirklich.“ Danach mit echtem Mausklick
  an vier Stellen: „Cover, Titel, Künstler und Leerraum → „Jetzt läuft““. Im Abschluss steht: „Kein `element.click()`,
  kein `userGesture`.“ Und eine eigene Liste „**Nicht gemessen** (geht nur mit dem echten, sichtbaren Fenster)“.
- **Bau 7** (Commit `4c50d6c`) prüfte das Abspielen ohne jede Markierung und schrieb selbst dazu, dass das nicht reicht:
  Der Testlauf ohne Fenster „beweist Punkt 1 nicht“ (sein Punkt 1 ist mein Punkt 5), „Beweis nur im echten Fenster.“
- **Meine nächste Abnahme** am selben Tag (Entscheidungs-Log, „Hauptfenster abgenommen bis auf Kleinigkeiten“):
  „**Musik startet** (Autoplay-Fix bestätigt).“ Die Suche ging im echten Fenster noch immer nicht, aus einem zweiten
  Grund; den behob ein Nachtrag zu Bau 8.
- **Am Abend** (Entscheidungs-Log, „MVP-Abschluss“): „Suche, Lautstärke, Songtext, Neustart nach X funktionieren“. Am
  2026-10-15 habe ich die erste fertige Fassung abgenommen.

Das Muster dahinter gilt im ganzen Team-OS: Ein Bericht ist keine Prüfung, und eine Prüfung ist keine Abnahme. Hier hat
erst die Abnahme gezeigt, was die Prüfung nicht sehen konnte, und die Regel ändert seitdem, wie geprüft wird.
