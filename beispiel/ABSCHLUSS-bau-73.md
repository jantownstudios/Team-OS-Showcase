# Abschluss Bau 73 · Notschalter „Einlese-Sperre“ im Dashboard (0.2.14)

Beginn 2026-11-10 11:45 · Ende 2026-11-10 12:07 · Runde: **klein** · Arbeitsort `rollen\bau` (Zweig `rolle/bau`, `main` gemergt auf `5bdbb50`) · uncommittet · nur Debug-Bau (`-j 4`, `--offline`) · Version nicht angefasst · keine neue Abhängigkeit · **0 `claude`-Aufrufe** · kein sichtbares Fenster (Bilder kopflos) · `F:\Team-OS`, `docs/design/**` nicht angefasst, Testdaten nur in `.pruefung/` · F: am Ende 25,7 GB frei.

## Bitte zuerst wissen

**Alle fünf Abnahmepunkte haben einen Test oder ein Bild, alles ist grün.** Geprüft habe ich mit der echten Pipe der App und dem echten Haken-Code. Ein echtes `claude` lief nicht.

**Bilder für Jan** (kopflos gerendert, Schein-Assistent):
- `F:\TeamOS-App\rollen\bau\.pruefung\bau73\dashboard-1920-an.png` und `…\dashboard-1920-aus.png`
- `F:\TeamOS-App\rollen\bau\.pruefung\bau73\dashboard-1280-an.png` und `…\dashboard-1280-aus.png`

**Der Platz ist rechts** in einem eigenen Block „MASTER · 1“ zwischen EXTERN und SPERRLISTE (Auslegung 1). Das ist das einzige Stück, das nach Design riecht. Bitte am Bild entscheiden.

## Was geändert ist

| Datei | Änderung |
|---|---|
| `crates/teamos/src/werkzeuge.rs` | `EINLESE_SPERRE` (Schlüssel in `dashboard.json`), Feld `einlese_aus`, `einlese_sperre()`, `einlese_sperre_kippen()`; Lesen und Schreiben; Test `einlese_sperre_notschalter` |
| `crates/teamos/src/dashboard.rs` | Block „MASTER“ mit der Zeile „Einlese-Sperre“; den globalen Schalter einer Zeile (Wort, NeoMaster, Schild, Fokus) als `schalter()` aus `zeile()` herausgelöst, damit beide denselben benutzen; `Tat::EinleseSperre` |
| `crates/teamos/src/oberflaeche.rs` | `werkzeuge_pflegen` und `master_anlegen` geben den Schalter an die Andockung jeder Master-Seite (2 Zeilen) |
| `crates/assistent/src/lib.rs` | `Assistent::einlese_sperre(an)`, Grundwert: nichts tun |
| `crates/assistent/src/claude/mod.rs` | `Innen.einlese_aus` (Grundwert `false` = Sperre an); Antwort auf `stand` und `einlesen_stop`; `einlese_sperre()` am `ClaudeAssistent`; Test `notschalter_einlese_sperre` |
| `crates/assistent/src/claude/einlesen.rs` | `Einlesen::stopp_ohne_sperre()`: durch, Zählung der Zurückweisungen auf 0 (7 Zeilen) |
| `crates/teamos/src/bin/pruefung_bau73/`, `pruefung.rs` | Prüfwerkzeug `… -- bau73` (Bilder, Klick, Datei, Neustart) |

## Wie es funktioniert

1. **Speicher.** `dashboard.json` im App-Datenordner bekommt `"einlese_sperre": true|false`.
   - **Nur ein gespeichertes `false` schaltet aus.** Fehlt die Datei oder der Schlüssel, ist die Datei kaputt oder unlesbar, oder steht dort etwas anderes als ein Wahrheitswert (`"aus"`, `0`, `null`, Objekt, Liste): Die Sperre ist **an**.
   - Ein unbekannter Wert bleibt in der Datei stehen, wenn die App aus anderem Grund schreibt. Erst Jans Klick ersetzt ihn.
   - Er ist **kein** Eintrag im `KATALOG` und **kein** `Werkzeug` im Stecker. `global_kippen`, `projekt_setzen` und `wie_global` kennen die ID nicht und tun nichts. Ausnahmen je Projekt gibt es nicht. Eine `werkzeuge.json` mit `einlese_sperre` wird nicht gelesen.
2. **Jans Klick.** Die Raute im Dashboard ruft `einlese_sperre_kippen()`. Das kippt den Wert, schreibt `dashboard.json` sicher und zählt `stand` hoch. Ein Bestätigen gibt es nicht, weil es keine Ausnahmen gibt, die gelöscht würden. Das ist dieselbe Regel wie bei einem globalen Schalter ohne Ausnahmen (§15.5).
3. **Weg zur Andockung.** `werkzeuge_pflegen` sieht den neuen `stand` und ruft für jede offene Master-Seite `assistent.einlese_sperre(wert)` auf. Eine neue Master-Seite bekommt den Wert in `master_anlegen` vor dem ersten Start. Die Andockung merkt ihn nur im Speicher (`Innen.einlese_aus`). Bis zum ersten Aufruf gilt dort „an“.
4. **Wirkung.**
   - Haken (`stand`): Bei einem Master ist `einlesen` dann `[]`. Bei einer Rolle bleibt es `null` wie bisher.
   - Stop-Hook (`einlesen_stop`): Die Antwort ist „durch“, und die Folge der Zurückweisungen beginnt neu.
   - Der Strom zählt die gelesenen Dateien unverändert weiter. Schaltet Jan wieder ein, antwortet die App mit dem echten Stand.
   - Haken, Stop-Hook und Einstellungen von `claude` sind unverändert. Weil beide bei jedem Aufruf die App fragen, wirkt der Schalter **sofort**, auch in laufenden Sitzungen.
   - Ist die App nicht erreichbar oder die Antwort unecht, bleibt die Sperre wie bisher im Haken an. Der Schalter öffnet nur über eine echte, unterschriebene Antwort.

## Auslegungen + Grund (zur Entscheidung)

1. **Platz: rechts, eigener Block „MASTER · 1“** zwischen EXTERN und SPERRLISTE.
   - Etikett-Zeile und Abstände (40 über dem Etikett, 16 bis zum Block) sind wie bei SPERRLISTE, die Zeile ist wie eine Werkzeug-Zeile ohne „Ausnahmen“ und „Zuletzt“.
   - Rechter Etikett-Text: „gilt für alle Master-Sitzungen, Rollen nie“.
   - **Grund:** Links stehen schon acht Zeilen, die Spalte ist deutlich länger als rechts. Bei 1920 × 1080 ist die Seite 1470 hoch, rechts endet der Inhalt nach zwei Sperr-Einträgen bei etwa 700. Unten in INTERN hätte sie den Unterschied noch vergrößert und läge erst nach dem Scrollen.
   - Unter EXTERN passt sie nicht, denn sie ist nicht von außen.
   - **Neu daran ist nur das Etikett „MASTER“.** Ein bestehendes Muster für eine einzelne Zeile ohne eigene Gruppe gab es nicht. Wenn Design einen anderen Platz oder Namen will, ist es ein Umzug von wenigen Zeilen.
2. **Zeichen: das des Wunsch-Logs (drei Zeilen).** Es steht für „Text lesen“. Ein eigenes Zeichen gibt es nicht, und das Erfinden gehört Design. Es gibt ein Vorbild für ein doppelt genutztes Zeichen: „Meldungen aufs Handy“ und „Handy-App“ teilen sich auch eines.
3. **Beschreibung:** „Master lesen nach jedem Start zuerst ihre Pflichtdateien ganz.“, wörtlich das Beispiel aus dem Handoff, eine Zeile.
4. **Schild beim Verweilen** auf der Raute: Es ist das bestehende Schild „Für alle n Projekte“. Es passt, weil der Schalter für alle Projekte gilt.
5. **Stop-Hook bei „aus“ setzt die Zählung auf 0.** Sonst hätte eine Zurückweisung vor dem Ausschalten nach dem Wiedereinschalten den Ausstieg vorgezogen. Dafür steht eine kleine Methode in `einlesen.rs`. Die Datei ist nicht ausdrücklich genannt, gehört aber zur „Antwort auf `einlesen_stop`“.
6. **Keine App-Nachricht an den Master** beim Umschalten (anders als bei Steckern). Der Handoff verlangt keine. Der Master merkt es daran, dass der Haken nicht mehr sperrt.
7. **Zahl in der Seitenleiste** („Dashboard 8“) und „INTERN · 8“ bleiben bei 8, weil die Einlese-Sperre kein Werkzeug der KI ist.

## Prüfung (eigene, nicht Jans Abnahme)

**Tests** (Protokolle in `.pruefung/bau/`):
- `cargo test -j 4 -p assistent --offline -- --skip zweites_claude --skip wurzel_chrome_und_node`: **110 bestanden, 0 fehlgeschlagen**, davon 1 neu (`tests-bau73-assistent.txt`).
- `cargo test -j 4 -p teamos --lib --offline -- dashboard werkzeuge master …`: **141 bestanden, 0 fehlgeschlagen**, davon 1 neu (`tests-bau73-teamos.txt`).
- `cargo clippy -p assistent -p teamos --tests --bins`: keine Warnung in den angefassten Dateien.
- Kein voller Lauf.

| Abnahme | Ergebnis | Beleg |
|---|---|---|
| 1 Aus in laufender Sitzung, wieder an | Master mit 9 fehlenden Pflichtdateien. Vorher `Write` = `deny`. **Aus:** `stand` → `einlesen: []`; `Write` geht ohne Rückfrage; `Bash`/`rolle_starten` ohne Einlese-Meldung; `git push` fragt weiter (`ask`, bisherige Regeln); Stop-Hook fünfmal „durch“, keine Zeile „Einlesen unvollständig“; ein `Read` von START.md zählt weiter. **Wieder an:** `stand` nennt die echten 8 fehlenden, `Write` = `deny` ohne START.md in der Liste; Stop-Hook wieder dreimal `block`, dann durch | `notschalter_einlese_sperre` (echte Pipe, echter Haken-Code) |
| 2 Fehlt, kaputt, unbekannt | Sperre an bei: Datei fehlt; `{kaputt`; leer; `[]`; Schlüssel fehlt; `"aus"`, `0`, `null`, Objekt, Liste; Wert nur unter `global`; Ordner statt Datei (unlesbar). Unbekannter Wert bleibt beim Schreiben stehen | `einlese_sperre_notschalter` |
| 3 Neustart | Klick → `false` in der Datei; neu gelesen: aus; wieder gekippt, neu gelesen: an. Im Prüfwerkzeug über den echten Klick auf die Raute, je Größe | `einlese_sperre_notschalter`, `… -- bau73` (`.pruefung/bau73/bericht.txt`, 0 Fehler) |
| 4 Kein anderer Weg | **Pipe:** Anfragen mit den Arten `einlese_sperre`, `einlesen_an`, `einlesen_aus`, `einlese_sperre_setzen`, `stecker` und Feldern `an`/`einlese_sperre`/`wert` ändern ihn in keiner Richtung (die App legt unbekannte Arten als offene Anfrage ab und antwortet nicht, darum mit Frist gefragt). **`werkzeuge.json`:** `{"ausnahmen": {"einlese_sperre": true}, "einlese_sperre": true}` im Projekt ändert nichts. **Andere Schreibwege:** `global_kippen`, `projekt_setzen`, `wie_global` mit der ID, andere Schalter, Sperrliste, „zuletzt“ ändern ihn nicht; der Stecker bleibt gleich. **Webapp:** Im Code gibt es keinen Weg dorthin. `einlese_sperre_kippen` ruft nur `dashboard.rs` (Klick). `crate::mobil*` hat keinen Zugriff auf `Werkzeuge`, und `dashboard.json` steht dort auf der Liste verbotener Dateien (`mobil/tests_bau61.rs`) | `notschalter_einlese_sperre`, `einlese_sperre_notschalter`, `grep` |
| 5 Bild beider Stellungen | 1920 × 1080 und 1280 × 720, je an und aus, Pfade oben | `cargo run -j 4 --bin teamos-pruefung -- bau73` |

## Offene Punkte

1. **Alter Prüfschritt Bau 40:** `pruefung_bau40` prüft „alle Orte `raute-*` gehören zum `KATALOG`“. Mit `raute-einlese_sperre` schlägt dieser eine Punkt jetzt fehl. Ich habe es nicht angepasst, weil das ein alter Bericht ist. Bei Bedarf dort `einlese_sperre` ausnehmen.
2. **Nachweis mit echtem `claude`** (Master mit Jan): Einen Master starten, Pflichtdateien nicht lesen lassen, im Dashboard ausschalten. Der nächste `Write` muss gehen, und die Antwort darf nicht zurückgehalten werden. Danach einschalten: wieder gesperrt.
3. **Laptop und Handy** haben den Schalter nicht (laut Handoff). Die Webapp zeigt ihn nicht.
4. **Zeichen** (Auslegung 2) und **Platz/Etikett** (Auslegung 1) zur Entscheidung an Design oder Master.
