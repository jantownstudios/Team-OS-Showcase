[English](README.md) · **Deutsch**

# Team-OS: ein Betriebssystem für ein KI-Team

**Jan Prügel** · AI Agent Builder · [GitHub](https://github.com/jantownstudios)

Ich führe mehrere KI-Sitzungen wie ein kleines Team: ein **Multi-Agent-System**. Für jedes Projekt plane ich mit
einem leitenden Agenten, dem **Master**. Spezialisierte Agenten, die **Rollen**, arbeiten nach schriftlichem Auftrag
im Hintergrund. Eine eigene Windows-App orchestriert sie alle und setzt die Regeln durch.

Dieses Repo ist ein **Schaufenster**: Es zeigt, was entstanden ist und wie gearbeitet wird. Der Code selbst ist
privat. Ein echtes, prüfbares Beispiel für eine Arbeitsrunde liegt in [`beispiel/`](beispiel/).

![Aufbau: Ich plane mit dem Master, der Master gibt den Rollen Aufträge, die App startet sie, stellt MCP-Werkzeuge bereit und prüft jeden Werkzeug-Aufruf mit Hooks, jede Sitzung läuft über Claude Code](bilder/aufbau.png)

## Auf einen Blick

| | |
|---|---|
| **Was** | ein festes Arbeitssystem für Software-Projekte mit KI-Agenten, dazu eine eigene Desktop-App, die es orchestriert und absichert |
| **Wer** | ich, als <!-- titel -->AI Agent Builder<!-- /titel -->, mit vielen Projekten gleichzeitig |
| **KI** | Claude Code (das offizielle Programm von Anthropic), ohne Fenster gestartet, mit meinem eigenen Abo |
| **App** | Windows, geschrieben in Rust, Oberfläche mit egui; dazu eine Web-Ansicht für Handy und Laptop |
| **Zeitraum** | App seit 2026-10-27, Stand dieser Seite 2026-11-10 |
| **Meine Rolle** | System entworfen, Entscheidungen getroffen, jede Lieferung abgenommen. **Den Code haben die KI-Rollen geschrieben.** |

## Was es ist

Eine KI-Sitzung vergisst alles, sobald sie endet. Und sie prüft ihre eigene Arbeit nicht zuverlässig. Das Team-OS
löst beides mit klaren Zuständigkeiten und mit Dateien als Gedächtnis.

| Begriff | Was das ist |
|---|---|
| **Master** | der leitende Agent eines Projekts (Orchestrator). Plant mit mir, schreibt Aufträge, prüft Ergebnisse. Baut selbst nichts, damit es immer einen zweiten Blick gibt. |
| **Rolle** | eine KI-Sitzung für genau einen Auftrag, danach endet sie. Im App-Projekt gibt es fünf: **Bau** (schreibt Code), **Design** (Aussehen), **Test** (probiert fremde Programme aus), **Gegenprobe** (versucht, die Sperren der App zu umgehen), **Recherche** (liest Fachquellen). |
| **Handoff** | der schriftliche Auftrag an eine Rolle: Ziel, meine Entscheidungen, was sie ändern darf und was nicht. |
| **Abschluss** | der Bericht der Rolle: was geändert ist, wie sie es geprüft hat, was offen bleibt. |
| **Kern** | eine kleine Sammlung von Regeln, nach denen alle Master gleich arbeiten. Jede Regel nennt den Vorfall, aus dem sie entstand. |
| **App** | startet und überwacht alle Sitzungen und setzt die Regeln technisch durch. |

Jede Rolle arbeitet in ihrer eigenen Kopie des Projekts (Git-Worktree). So können mehrere Rollen gleichzeitig
arbeiten, ohne sich in die Quere zu kommen.

## Meine Rolle, ehrlich

| Ich | Die KI-Rollen |
|---|---|
| habe das System entworfen: Regeln, Rollen, Abläufe, Prüfstufen | schreiben den Code, die Tests und die Gestaltung |
| entscheide, was gebaut wird und in welcher Reihenfolge | arbeiten genau den Auftrag ab, den der Master schreibt |
| beantworte Freigaben (zum Beispiel: darf diese Rolle starten?) | fragen nach, wenn eine Sperre sie aufhält |
| nehme jede sichtbare Änderung am echten Fenster ab | berichten im Abschluss, was sie selbst geprüft haben |

Ich programmiere die App nicht Zeile für Zeile. Meine Arbeit ist das **System dahinter**: wie Aufgaben geschnitten
werden, wer was darf, wie geprüft wird, und wann etwas fertig ist.

## Wie gearbeitet wird

| Schritt | Wer | Was passiert |
|---|---|---|
| 1 · Plan | ich und Master | Wir entscheiden, was als Nächstes kommt. Die Entscheidung steht mit Datum in einer Datei. |
| 2 · Auftrag | Master | schreibt den Handoff für eine Rolle. |
| 3 · Start | ich | Ein Klick auf „Erlauben“ startet die Rolle. Ohne Klick startet nichts. |
| 4 · Lieferung | Rolle | arbeitet im Hintergrund und schreibt einen Abschluss. |
| 5 · Prüfung | Master | glaubt dem Bericht nicht, sondern prüft selbst: Tests, Änderungen lesen, eigene Bilder. |
| 6 · Gegenprobe | Rolle Gegenprobe | versucht bei Sicherheitsthemen gezielt, die Sperren zu umgehen. Findet sie einen Weg, geht es zurück an Bau. |
| 7 · Abnahme | ich | schaue mir das Ergebnis am echten Fenster an. |
| 8 · Auslieferung | Master | erst nach meinem Go: neue Version, Installationspaket, Notizen für alle Master. |

**Warum so viele Stufen?** Ein Bericht ist keine Prüfung, und eine Prüfung ist keine Abnahme. Jede Stufe fängt
andere Fehler. „Alle Tests grün“ allein zählt bei uns nicht als geprüft. Diese Regeln stammen aus echten Vorfällen
in früheren Projekten.

**Selbst nachsehen:** In [`beispiel/`](beispiel/) liegen der echte Handoff und der echte Abschluss der Runde, die die
erste Fassung dieses Schaufensters gebaut hat.

## Was die App kann

Eine Auswahl. Der Fachbegriff steht in Klammern.

| Funktion | Kurz |
|---|---|
| **Rollen starten und überwachen** (Orchestrierung, MCP-Werkzeuge `rolle_starten`, `rolle_abbrechen`) | Der Master startet Rollen selbst über ein Werkzeug der App, ich bestätige per Klick. Ein Stern je Rolle zeigt, wer gerade arbeitet. Ich kann jede Rolle anhalten. |
| **Eigene Werkzeuge für die Agenten** (eigener MCP-Server) | Die App bringt einen eigenen MCP-Server mit. Master und Rollen rufen seine Werkzeuge auf: Rolle starten oder anhalten, einem anderen Projekt schreiben, das eigene Postfach lesen, Fehler oder Wünsche melden (`wunsch_melden`), den Browser bedienen. |
| **Freigaben per Klick** (Human in the Loop, Permission-Prompts als Pop-up) | Heikle Schritte (Rolle starten, Löschen, Hochladen) warten auf mein „Erlauben“, am PC, am Handy oder am Laptop. |
| **Prüfung jedes Werkzeug-Aufrufs** (Hooks `PreToolUse`, `Stop`) | Bevor ein Werkzeug läuft, fragt ein Hook die App: Ist das Werkzeug abgeschaltet, steht die Adresse auf der Sperrliste, braucht diese Datei meine Freigabe? Hängt oder stürzt der Hook ab, bleibt das Werkzeug gesperrt (fail closed). Ein `Stop`-Hook schickt einen frischen Master zurück, wenn er antworten will, bevor er seine Pflichtdateien gelesen hat. |
| **Dashboard** (Werkzeug-Rechte je Projekt) | Ich schalte einzelne Werkzeuge der KI ab, für alle Projekte oder nur für eines. Was aus ist, fehlt der KI ganz. |
| **Eigener Browser** (Browser-Automatisierung über MCP-Werkzeuge `browser_oeffnen`, `browser_lesen`, `browser_klicken` …, Sperrliste für Adressen) | Die KI surft in einem Browser innerhalb der App, mit einer Sperrliste für Adressen. Bevor sie ein Formular abschickt, fragt die App mich. |
| **Funk zwischen Mastern** (Nachrichten zwischen Agenten, MCP-Werkzeuge `funk_senden`, `post_lesen`) | Ein Master kann einem anderen Projekt eine Aufgabe schicken. Ich sehe jede Nachricht wörtlich, bevor sie rausgeht. |
| **Master-Wechsel** (Kontext-Verwaltung) | Wird das Gedächtnis eines Masters voll, sichert er seinen Stand in eine Datei, und ein frischer Master übernimmt. |
| **Handy und Laptop** (Web-App, Web-Push) | Projekte, Freigaben und Chat auch unterwegs; das Team Center zeigt alle laufenden Master und Rollen auf einer Seite. |

### Bilder

Alle Bilder stammen aus den automatischen Prüfungen der App und zeigen **ausgedachte Testdaten**.

**Master-Seite:** links Navigation und Post, in der Mitte der Chat mit dem Master, rechts die Rollen. Hier fragt die
App, wie die Rolle „Bau“ aufhören soll.

![Master-Seite mit Karte zum Anhalten einer Rolle](bilder/01-master-seite.png)

**Dashboard:** Werkzeuge der KI an- und abschalten, Sperrliste für Adressen.

![Dashboard mit Schaltern für Werkzeuge und Sperrliste](bilder/02-dashboard.png)

**Team Center am Laptop:** alle Master, laufenden Rollen und Nachrichten zwischen Projekten auf einer Seite.

![Team Center mit Mastern, Rollen, Limits und Postfach](bilder/03-team-center.png)

**Roadmap am Laptop:** der Plan eines Projekts, mit der Rolle, die gerade an einem Schritt arbeitet.

![Roadmap eines Projekts mit laufenden und geplanten Schritten](bilder/04-roadmap.png)

**Freigabe am Handy:** Eine Rolle möchte einen Befehl ausführen und wartet auf „Erlauben“ oder „Ablehnen“.

<img src="bilder/05-freigabe-handy.png" alt="Freigabe-Karte im Chat am Handy" width="320">

## Zahlen

Stand 2026-11-10. Gezählt aus der Git-Historie (Versionen, Commits) und den nummerierten Handoffs (Runden je Rolle);
Tests aus dem letzten vollen Testlauf, Projekte aus der Projektliste der App.

| | |
|---|---|
| Start der App | 2026-10-27 |
| Versionen | **29**, von 0.1.0 (2026-10-28) bis 0.2.14 (2026-11-10) |
| Commits im Code | 491 |
| Runden je Rolle | Bau 73 · Design 53 · Gegenprobe 29 · Test 10 · Recherche 13 |
| Tests mit mir | 6, am echten Handy, Laptop und PC |
| Automatische Tests | 708 im letzten vollen Lauf |
| Projekte in der App | rund 29 |

Über das ganze Team-OS, alle Projekte zusammen (Stand 2026-10-31):

| | |
|---|---|
| Projekte mit eigenem Code-Repo | 22 |
| Aufträge an Rollen (Handoffs) | 532 |
| Entscheidungen mit Datum und Begründung | 351 |

## Technik

| | |
|---|---|
| Sprache | Rust |
| Oberfläche | egui (Desktop), HTML/CSS/JavaScript (Handy und Laptop) |
| Agenten | Claude Code, von der App ohne Fenster gestartet (`claude -p`, `stream-json`), ein Prozess je Sitzung; kein eigener Zugang, kein API-Schlüssel |
| Werkzeuge | eigener MCP-Server der App; Hooks (`PreToolUse`, `Stop`) an jeder Sitzung |
| Zusammenarbeit | Git; jede Rolle in eigener Arbeitskopie (Worktree) |
| Lokal | Alles läuft auf meinem PC; Handy und Laptop greifen über einen eigenen verschlüsselten Zugang darauf zu. Keine Cloud-Dienste, keine Telemetrie. |

---

Alle Rechte vorbehalten. Text und Bilder nur zur Ansicht.
