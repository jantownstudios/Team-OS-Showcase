# Real examples: a round of work and a rejected delivery

Two examples. The first is the real documents of one round, copied from the private code repo, **unchanged except
for redactions**: a **handoff** (the task brief the Master wrote for a role) and a **completion report** (*Abschluss*:
what the role changed, how it checked its own work, what is still open). The second is a case I wrote up from the
sources of another project: a delivery that two roles reported as checked and that I did not sign off.

## 1 · Build 73: a feature with acceptance tests

Brief → implementation → each acceptance point backed by a test.

**Which round:** Build 73, on 2026-11-10, role *Bau* (Build). The feature is a kill switch for the read-in lock
(*Einlese-Sperre*): a safeguard that makes a fresh Master read its required files before it may act. If the lock blocks
by mistake, I can switch it off in the dashboard with one click. You can see the switch in the
[dashboard screenshot](../bilder/02-dashboard.png), block „MASTER“ on the right.

The handoff sets five acceptance points, each to be measured with a test:

| # | Acceptance point (handoff, „Prüfen“) | Evidence in the completion report |
|---|---|---|
| 1 | Off: a running Master session may use its tools at once; on again: the lock applies with the real state | table under „Prüfung“, row 1: test `notschalter_einlese_sperre` |
| 2 | Stored state missing, broken or unknown: the lock stays on | row 2: test `einlese_sperre_notschalter` |
| 3 | The switch survives a restart of the app | row 3: same test plus the check tool `bau73` |
| 4 | Nothing but my click changes it (pipe, project file, web app) | row 4: both tests plus a code search |
| 5 | A screenshot of both positions | row 5: check tool `bau73` |

The test counts are above that table (110 and 141 passed, 0 failed).

## 2 · A delivery I rejected

The README says „a report is not a review, and a review is not a sign-off“. This case shows the sign-off failing a
delivery whose reports said it was checked.

**Which rounds:** Design 7 and Build 6, on 2026-10-14, in a desktop player of mine for a music streaming service. Both
roles reported the main window as checked. In the real window I found eight points, four of them bugs; two of those
the roles had reported as checked. The Master measured why the checks missed them (a test mark that made inputs look
human, a click in code instead of at the spot), and a new rule for all roles came out of it the same night.

The case follows that order: what was reported as checked → what I found → why the checks missed it → the new rule,
word for word → what changed afterwards. Every statement names its source (file, date, commit). The project and the
service are not named, and one point that concerns signing in to the service is only counted.

## Numbers in these documents

Each document is the state of its own day. The main README counts again as of a later day, so numbers there can be
higher.

## Files

Everything was written in German, the working language of the whole system. The English versions were translated
afterwards for readers; the German files are the originals.

| File | Language | Translation |
|---|---|---|
| [`HANDOFF-bau-73.md`](HANDOFF-bau-73.md) | German (original) | [`HANDOFF-bau-73.en.md`](HANDOFF-bau-73.en.md) |
| [`ABSCHLUSS-bau-73.md`](ABSCHLUSS-bau-73.md) | German (original) | [`ABSCHLUSS-bau-73.en.md`](ABSCHLUSS-bau-73.en.md) |
| [`fall-abnahme.de.md`](fall-abnahme.de.md) | German (original) | [`fall-abnahme.md`](fall-abnahme.md) |

**Redacted:** wherever a text named my domain, an address in my home network or a server provider, it now says
`[entfernt]` ("removed"). Build 73 needed no redaction. Everything else is as written, including file paths on my
PC and my first name. The case in example 2 is not a copy but a write-up; its quotes are word for word.
