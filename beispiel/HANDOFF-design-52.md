# Handoff: Design 52 · Schaufenster für Bewerbungen: README, Schaubild, Bilder — Rolle „Design“

Stand: 2026-11-10 · Auftraggeber: Master · Abschluss nach: `docs/ABSCHLUSS-design.md` (überschreiben; Vorgänger Design 51, in Git) · Arbeitsort: **`rollen\design`** (vorher `git merge main`) · Modus: **Umsetzung (Canvas: nein)** · Runde: **mittel** · Nichts committen.

## Worum es geht
Jan bewirbt sich und will Recruitern (HR, keine Entwickler) zeigen, was er gebaut hat. Dafür kommt ein **eigenes öffentliches Repo** `jantownstudios/Team-OS-Showcase`. Es enthält nur eine README mit Bildern und einem Schaubild. Das Code-Repo `TeamOS-App` bleibt privat. Du lieferst den Inhalt, der Master prüft ihn, legt das Repo an und veröffentlicht es mit Jans Go.

## Jans Entscheidungen (2026-11-10)
- **Eigenes Schaufenster-Repo**, `TeamOS-App` bleibt privat.
- **Deutsch.**
- **Kein Code** im Schaufenster, nur README, Bilder, Schaubild, Zahlen.
- **Name:** `Team-OS-Showcase`.

## Vorher lesen (nur lesen, nichts dort schreiben)
- `F:\Team-OS\START.md`, `F:\Team-OS\kern\*.md` — wie das System arbeitet (Master, Rollen, Handoff, Abschluss, Prüfstufen).
- `F:\Team-OS\projects\teamos-app\PATCHNOTES.md` — was die App kann.
- `F:\Team-OS\projects\teamos-app\analyse\A-aufbau.md`, `B-abgleich.md`, `C-potenzial.md`, `Z-zahlen.md`, `G-begriffe.md` — fertige Beschreibung und Einordnung.
- `docs/design/SPEZIFIKATION.md` und `tokens.json` — Look und Farben.

## Auftrag
1. **`schaufenster/README.md`** in deinem Arbeitsort, auf Deutsch, für HR lesbar (keine Fachwörter ohne Erklärung, kurze Absätze, Tabellen statt Fließtext, wo es geht). Inhalt:
   - **Was es ist:** ein Betriebssystem für ein KI-Team. Je Projekt plant ein **Master** mit Jan, **Rollen** (Bau, Design, Test, Gegenprobe, Recherche) arbeiten nach schriftlichem Auftrag im Hintergrund, alles läuft in einer eigenen Windows-App (Rust).
   - **Jans Rolle, ehrlich:** Jan hat das System entworfen (Regeln, Rollen, Abläufe, Prüfstufen), trifft die Entscheidungen und nimmt jede Lieferung ab. **Den Code schreiben die KI-Rollen.** Nicht beschönigen, nicht kleinreden.
   - **Wie gearbeitet wird:** Plan → Auftrag (Handoff) → Rolle liefert mit Abschluss → Master prüft selbst → Gegenprobe versucht die Sperren zu umgehen → Jan nimmt am Bild ab → Auslieferung. Warum: Bericht ≠ Prüfung ≠ Abnahme.
   - **Was die App kann:** Auswahl, kurz (Rollen starten und überwachen, Freigaben per Klick, Dashboard zum Abschalten von Werkzeugen, eigener Browser mit Sperrliste, Funk zwischen Mastern, Handy- und Laptop-Zugang). **Keine Wege an Sperren vorbei, keine Schwachstellen, keine Gegenproben-Befunde im Einzelnen.**
   - **Zahlen** (vom Master gemessen, so übernehmen): Start 2026-10-27 · **29 Versionen** von 0.1.0 (2026-10-28) bis 0.2.14 (2026-11-10) · 488 Commits · Runden: Bau 73, Design 51, Gegenprobe 29, Recherche 13, Test 17 · 708 automatische Tests im letzten vollen Lauf · rund 29 Projekte in der App. Mehr Zahlen nur aus `Z-zahlen.md`, mit Datum.
   - **Bilder** (Punkt 3) und **Schaubild** (Punkt 2) eingebunden, relative Pfade.
2. **Schaubild** `schaufenster/bilder/aufbau.png` (und `.svg`, wenn dein Werkzeug es kann): Jan · Master · Rollen · App (Freigaben, Wächter, Dashboard) · Claude. Im Souli-Look (Tokens). Lesbar auf GitHub hell und dunkel (eigener Hintergrund im Bild). Werkzeug unter `docs/design/werkzeug/design52.py`.
3. **Bilder der App**, 3 bis 5, unter `schaufenster/bilder/`. Nimm sie aus den Prüfmodi, die mit **Testdaten** arbeiten (`cargo run -j 4 --bin pruefung -- <modus>`, z. B. `bau69`, `bau62`, `bau55`; Rust-Umgebung wie in `docs/ROLLEN.md`; nur Debug-Bau) oder aus `docs/design/entwuerfe/`. Jedes Bild **selbst ansehen** und prüfen: keine echten Daten von Jan, keine Adressen, keine Domain, keine IP, keine Namen außer Projektnamen aus Testdaten. Bild, das du nicht sicher prüfen kannst, nicht nehmen.
4. **Prüfliste** im Abschluss: je Datei im Ordner `schaufenster/` ein Satz, warum sie öffentlich sein darf. Dazu eine Suche über den ganzen Ordner nach `[entfernt]`, `[entfernt]`, `[entfernt]`, `[entfernt]`, `@`, `C:\`, `F:\`, `Users` mit Ergebnis.
5. **Offene Fragen** an den Master knapp, je mit Empfehlung (z. B. Kontaktzeile, Name des Autors, Lizenz-Hinweis).

## Grenzen
- Nur `schaufenster/**` in deinem Arbeitsort, `docs/design/werkzeug/design52.py`, Ausgaben der Prüfmodi in `.pruefung/` und dein Abschluss. Kein App-Code, keine SPEZ, keine Tokens ändern.
- Keine persönlichen Daten von Jan (Name, Mail, Wohnort, Gesundheit). Keine Heimnetz-Angaben, keine Server-Anbieter, keine Domain.
- Keine fremden Texte oder Tabellen kopieren; Kern und Analyse nur zusammenfassen.
- Kein sichtbares Fenster, kein Repo anlegen, nichts veröffentlichen, kein `gh`.

## Dateien
- **Darf:** `schaufenster/**`, `docs/design/werkzeug/design52.py`, `.pruefung/**`, `docs/ABSCHLUSS-design.md`
- **Darf nicht:** alles andere
