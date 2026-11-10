# Abschluss: Design 52 · Schaufenster für Bewerbungen: README, Schaubild, Bilder

Rolle: Design · Runde: mittel · Modus: Umsetzung (Canvas: nein) · **Beginn 2026-11-10 15:11 · Ende 2026-11-10 15:24** (Uhr des Rechners)
Arbeitskopie `F:\TeamOS-App\rollen\design` (Zweig `rolle/design`, `main` hineingeführt auf `4ee9f3c`). Nichts committet, kein Repo angelegt, kein `gh`. Handoff `docs/HANDOFF-design-52.md`.

**Ergebnis:** Der Inhalt für `Team-OS-Showcase` liegt in `rollen\design\schaufenster\`: eine README auf Deutsch, ein Schaubild (PNG + SVG) und fünf Bilder der App aus Prüfmodi mit Testdaten. Die Suche nach den acht Wörtern findet nichts. Offen sind vier Fragen, vor allem die Ich-Form (Auslegung 1).

## Was geändert ist
| Datei | Was |
|---|---|
| `schaufenster/README.md` | neu: Einstieg, Auf einen Blick, Was es ist (Begriffe), Meine Rolle (ehrlich), Wie gearbeitet wird (8 Schritte + Warum), Was die App kann (7 Funktionen), Bilder, Zahlen, Technik |
| `schaufenster/bilder/aufbau.png` (1600 × 1000), `aufbau.svg` | neu: Schaubild Mensch · Master · Rollen · App mit Freigaben, Wächter, Dashboard · Claude |
| `schaufenster/bilder/01-master-seite.png` | Master-Seite mit Karte „Wie soll Bau 33 aufhören?“ (aus `bau33`, `zustand1-1920`, auf 1600 verkleinert) |
| `schaufenster/bilder/02-dashboard.png` | Dashboard (aus `bau73`, `dashboard-1920-an`, unten abgeschnitten, 1600 breit) |
| `schaufenster/bilder/03-team-center.png` | Team Center in der Laptop-Ansicht (aus `bau69`, `tc-1366`, unten abgeschnitten) |
| `schaufenster/bilder/04-roadmap.png` | Roadmap in der Laptop-Ansicht (aus `bau69`, `rm-1366`, unten abgeschnitten) |
| `schaufenster/bilder/05-freigabe-handy.png` | Freigabe-Karte im Chat am Handy (aus `bau56`, `offen`, 786 × 1704) |
| `docs/design/werkzeug/design52.py` | neu: malt das Schaubild, schneidet die Bilder zu, sucht die Wörter, schreibt `.pruefung/design52-pruefung.json` |

## Wie es funktioniert
- **Werkzeug:** `python docs/design/werkzeug/design52.py` aus dem Repo-Ordner. Vorher müssen die Prüfmodi gelaufen sein
  (`cargo run -j 4 --bin teamos-pruefung -- bau33 | bau73 | bau69 | bau56`, Debug-Bau). Die Liste `AUSWAHL` nennt je
  Bild Quelle, Zuschnitt mit Grund und Breite. Gespeichert wird mit PIL ohne Text-Metadaten.
- **Schaubild:** Das SVG wird aus den Tokens gebaut: `farbe.grund` mit Sonnen- und Mondlicht, Glas (Milch 0,05,
  Kante 0,14, Lichtkante 0,16), `radius.karte` 18. Die Rollenfarben folgen `farbe.rolle.reihenfolge`, also dem Platz in
  `ROLLEN.md`. Die Master-Kante läuft im Verlauf Sonne → Mond wie „Brainstorm“. Das PNG macht Chrome ohne Fenster in 1×
  mit Manrope aus `assets/schriften`. Der Hintergrund ist im Bild, so steht es auf GitHub hell und dunkel gleich.
  Schriftgrößen ≈ 1,5 × App-Maß, weil GitHub das Bild bei rund der halben Breite zeigt.
- **Suche:** Alle Dateien unter `schaufenster/` als UTF-8 und UTF-16. Bei PNG zählen nur die Blöcke, in denen Text stehen
  kann (`tEXt`, `zTXt`, `iTXt`, `eXIf`). In den Bildpunkten und im Kopf kommt „@“ zufällig vor (Breite 1600 = hex
  `06 40`, und `0x40` ist „@“). Das war der erste Fehlalarm, siehe Prüfung.
- **Zweimal gleich:** Drei Läufe hintereinander ergaben dieselben Prüfsummen aller sieben Dateien.

## Auslegungen + Grund (zur Entscheidung)
1. **README und Schaubild in der Ich-Form, ohne den Namen „Jan“.** Grund: Das Repo ist Jans eigenes Schaufenster, also
   spricht ein Bewerber selbst. So steht auch kein Name im öffentlichen Text („keine persönlichen Daten: Name“). Im
   Schaubild heißt der Mensch „Ich · Entwickler“. Soll es „Jan“ heißen, sind es 7 Texte in `design52.py` und die README.
2. **Bild 1 zeigt das Anhalten einer Rolle**, nicht den Master-Wechsel (`bau43`, ebenfalls sauber). Grund: Darin sind
   Chat, Rollen-Sterne und eine Entscheidungskarte zu sehen, und damit die Kernidee „Mensch entscheidet“.
3. **Zuschnitte:** Beim Dashboard stand unten links der Team-OS-Pfad (`F:\…`), in Team Center und Roadmap unten links
   der Gerätename mit Jans Vornamen. Beides ist weggeschnitten, nichts wurde übermalt.
4. **Roadmap aus der 1366er-Fassung:** Sie ist im Prüfmodus mittig gescrollt (die oberste Zeile ist halb angeschnitten).
   Die 1920er zeigt „wartet auf Jan“ und einen Fokusrahmen am Zurück-Knopf. Deshalb nehme ich die 1366er.
5. **Freigaben in der README:** „Hochladen“ steht für `git push`, damit HR es versteht. Die Gegenprobe ist nur als Rolle
   beschrieben („versucht, die Sperren zu umgehen“), ohne Befunde und ohne Grenzen der Sperren.
6. **Zahlen:** Die Zahlen des Masters stehen unverändert da. Aus `Z-zahlen.md` kommen nur drei, mit „Stand 2026-10-31“
   (22 Projekte mit Repo, 532 Handoffs, 351 Entscheidungen). Die Zeilenzahl aus `A-aufbau.md` ist nicht übernommen, weil
   der Handoff nur `Z` erlaubt.
7. **Wächter im Schaubild:** Er steht für Wächter und Haken zusammen („prüft jeden Werkzeug-Aufruf, meldet hängende
   Rollen“). Zwei Kästen hätten HR nur ein Fachwort mehr gebracht.

## Prüfung (eigene, nicht Jans Abnahme)
**Prüfliste je Datei:**
| Datei | Warum sie öffentlich sein darf |
|---|---|
| `README.md` | Sie fasst Kern, Patchnotes und Analysen in eigenen Worten zusammen, ohne kopierte Tabellen. Es gibt keinen Namen, keine Pfade, Adressen, Domains, Anbieter oder Heimnetz-Angaben und keine Sperren-Befunde. Die Zahlen stammen vom Master oder aus `Z`, jeweils mit Datum. |
| `bilder/aufbau.png`, `aufbau.svg` | Sie sind selbst gezeichnet und enthalten nur Begriffe des Systems. Kein Name, kein Logo eines fremden Programms (Claude nur als Wort). |
| `bilder/01-master-seite.png` | Testprojekt „probe“, Rollen Bau/Design/Test, Limits „noch kein Stand“. Das Bild ist angesehen und enthält keinen Pfad und keinen Namen. |
| `bilder/02-dashboard.png` | Die Sperrliste zeigt nur Beispiele aus dem Prüfmodus (`example.org/geheim`, `paypal.com`). Der Pfad unten ist abgeschnitten, das Bild ist angesehen. |
| `bilder/03-team-center.png` | Testprojekte kaderplaner, notizen, teamos-app, erfundene Post. Der Gerätename ist abgeschnitten, das Bild ist angesehen. |
| `bilder/04-roadmap.png` | erfundene Plan-Zeilen aus `bau69`. Der Gerätename ist abgeschnitten, das Bild ist angesehen. |
| `bilder/05-freigabe-handy.png` | Testprojekt teamos-app, erfundener Chat, Befehl `cargo test … lauf_7`. Das Bild ist angesehen. |

**Suche** über den ganzen Ordner nach `[entfernt]`, `[entfernt]`, `[entfernt]`, `[entfernt]`, `@`, `C:\`, `F:\`, `Users`
(UTF-8 und UTF-16; bei PNG die Text-Blöcke, vorhanden sind nur `IHDR`, `IDAT`, `IEND`): **keine Funde**
(`.pruefung/design52-pruefung.json`). Im ersten Lauf meldete die Suche „@“ in allen PNG. Das lag an Bildpunkten und Breite,
nicht an Text, und ist im Werkzeug behoben.

**Nicht genommen:** `bau40/linie-*` zeigt einen Benutzerpfad mit Namen, `bau45` einen QR-Code mit Adresse, `bau31` liest
echte ROADMAPs, `bau55` hat einen leeren Chat.

**Prüfmodi:** `bau73`, `bau69`, `bau43`, `bau33`: 0 Fehler. `bau55` meldet 2 Fehler, `bau40` 2 und `bau56` eine Szene mit
Fehlern. Alle drei sind ältere Modi gegen neueren Code; aus ihnen ist nur `bau56/offen` übernommen, und das Bild ist
angesehen und vollständig. Nicht untersucht, nicht meine Runde.

**Nicht belegt:** wie GitHub die README tatsächlich zeigt (hell/dunkel, Breite). Das SVG ohne eingebettete Schrift zeigt
dort Segoe UI/Helvetica statt Manrope. Die README bindet deshalb das PNG ein.

## Offene Punkte (Fragen an den Master, je mit Empfehlung)
1. **Ich-Form oder „Jan“?** Empfehlung: Ich-Form lassen. So spricht der Bewerber selbst, und kein Name steht im Repo.
   Der Name steht ohnehin am GitHub-Konto und in der Bewerbung.
2. **Kontaktzeile?** Empfehlung: keine in der README. Den Kontakt trägt die Bewerbung. Will Jan eine, dann nur ein Link
   auf sein GitHub-Profil, keine Mail (die Suche nach „@“ bleibt dann sauber).
3. **Lizenz-Hinweis?** Empfehlung: eine Zeile „Alle Rechte vorbehalten, Bilder und Text nur zur Ansicht“ und keine offene
   Lizenz, weil es kein Code zum Nachnutzen ist. Manrope ist im PNG nur gerendert, nicht weitergegeben. Das SVG bettet sie
   nicht ein (OFL-Hinweis wäre sonst Pflicht).
4. **SVG mitliefern?** Empfehlung: ja, als Zugabe. Eingebunden ist das PNG, weil GitHub im SVG keine Schrift nachlädt.
- Am Rande: `docs/design/werkzeug/__pycache__/` lag schon vor dieser Runde ungetrackt in der Arbeitskopie (aus früheren
  Werkzeug-Läufen). Ich habe es nicht angefasst.
