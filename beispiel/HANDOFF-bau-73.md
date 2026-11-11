# Handoff: Bau 73 · Notschalter „Einlese-Sperre“ im Dashboard (0.2.14) — Rolle „Bau“

Stand: 2026-11-10 · Auftraggeber: Master · Abschluss nach: `docs/ABSCHLUSS-bau.md` (überschreiben; Vorgänger Bau 72, in Git) · Arbeitsort: **`rollen\bau`** (vorher `git merge main`) · Runde: **klein** · Nichts committen.

Vorher lesen: `docs/ABSCHLUSS-bau.md` (Bau 72), `docs/HANDOFF-bau-71.md` (Entscheidungen), `crates/teamos/src/dashboard.rs` (Kopf, Zeile eines Werkzeugs), `crates/teamos/src/werkzeuge.rs`, `crates/assistent/src/claude/mod.rs` (Pipe-Arten `stand` und `einlesen_stop`), `docs/design/SPEZIFIKATION.md` §15 (nur lesen).

## Ziel
Jan kann die Einlese-Sperre im Dashboard mit einem Klick abschalten, falls sie fälschlich blockt. Die Wirkung tritt sofort ein, auch in laufenden Master-Sitzungen, ohne Neuinstallation.

## Entscheidungen von Jan (2026-11-10)
- „den Schalter in das Dashboard noch mit reinknallen“, und zwar **in 0.2.14** (er hat noch nicht aktualisiert). Die Version bleibt 0.2.14.

## Vom Master festgelegt
- **Ein globaler Schalter**, Standard **an**. Keine Ausnahmen je Projekt: Die liegen in `.teamos\werkzeuge.json` im Code-Ordner, und ein Master soll seine Sperre nicht über eine Datei in seinem Bereich beeinflussen können.
- **Die Richtung bei Fehlern bleibt sicher:** Ist der gespeicherte Stand fehlend, kaputt oder unlesbar, gilt die Sperre als **an**. Achtung: Die bestehenden Stecker kennen die umgekehrte Richtung („unlesbar = alles Abschaltbare aus“). Darum **nicht** als `Werkzeug` in `Stecker.aus` bauen, sondern so, dass ein Fehler die Sperre nie aufhebt.
- **Nur Jans Klick in der Oberfläche der App ändert ihn**, gespeichert nur im Datenordner der App (wie `dashboard.json`). Kein Weg über Pipe, Werkzeug-Server, Webapp oder eine Datei im Projekt.
- **Wirkung in der App:** Ist der Schalter aus, antwortet die App auf `stand` mit `einlesen: []` und auf `einlesen_stop` mit „durch“. Gezählt wird im Strom weiter, damit beim Wiedereinschalten der echte Stand gilt. Haken und Stop-Hook bleiben unverändert.
- **Aussehen:** wie eine bestehende Werkzeug-Zeile im Dashboard (Zeichen, Name „Einlese-Sperre“, globaler Schalter, eine Zeile Beschreibung, z. B. „Master lesen nach jedem Start zuerst ihre Pflichtdateien ganz“), aber ohne „Ausnahmen“ und „Zuletzt“. Den Platz wählst du so, dass beide Spalten weiter ausgewogen enden. Kein neues Aussehen erfinden; wenn kein bestehendes Muster passt, schreib es in den Abschluss.
- Laptop und Handy bekommen den Schalter in dieser Runde nicht.

## Grenzen
Nur dieser Schalter. Rollen bleiben ohne Einlese-Sperre. Kein echtes `claude`, kein sichtbares Fenster, keine echten Daten von Jan, nichts im Team-OS. Version nicht ändern. Keine neue Abhängigkeit. Tests passend zur Änderung (`assistent`, `teamos` mit `dashboard`, `werkzeuge`, `master`), `-j 4`, Ausnahmen aus `docs/ROLLEN.md`; kein voller Lauf. Bau-Ordner nicht leeren.

## Dateien
- **Darf:** `crates/teamos/src/dashboard.rs`, `crates/teamos/src/dashboard/**`, `crates/teamos/src/werkzeuge.rs`, wo nötig die Stelle in `crates/teamos/src/**`, die den Stand an die Andockung gibt, `crates/assistent/src/**` (nur Antwort auf `stand`/`einlesen_stop` und die Übergabe des Schalters), Tests, Prüfwerkzeug, `docs/ABSCHLUSS-bau.md`, `.pruefung/bau/`
- **Darf nicht:** `docs/design/**`, `docs/ROLLEN.md`, `Cargo.toml`, `F:\Team-OS`, Git

## Prüfen (Abnahme)
Gemessen, je mit Test:
1. Schalter aus: Eine laufende Master-Sitzung mit fehlenden Pflichtdateien darf sofort alle Werkzeuge nach den bisherigen Regeln, der Stop-Hook lässt durch. Wieder an: Die Sperre gilt mit dem echten Lese-Stand.
2. Gespeicherter Stand fehlt, ist kaputt oder hat einen unbekannten Wert: Die Sperre ist an.
3. Nach einem Neustart der App bleibt der Schalter, wie Jan ihn gestellt hat.
4. Kein anderer Weg als Jans Klick ändert ihn (Pipe-Anfragen, `werkzeuge.json`, Webapp-Routen).
5. Ein Bild des Dashboards mit dem neuen Schalter in beiden Stellungen (unsichtbar aufgenommen, Pfad im Abschluss), damit Jan es ansehen kann.
