# Real examples: two rounds of work

These files are the real documents of two rounds. They are copied from the private code repo, **unchanged except
for redactions**. Each round has a **handoff** (the task brief the Master wrote for a role) and a **completion report**
(*Abschluss*: what the role changed, how it checked its own work, what is still open).

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

## 2 · Design 52: the round that built this showcase

**Which round:** Design 52, on 2026-11-10, role *Design*. It built the first version of this showcase: README,
architecture diagram, screenshots. The Master then reviewed it, changed two lines, and published it after my go. The
handoff says openly that the showcase is meant for job applications.

## Numbers in these documents

Each document is the state of its own day. The main README counts again, so some numbers differ: design rounds have
continued since then (the Design 52 handoff says „Design 51“, the README counts later rounds). That handoff also says
„Test 17“; in the README, *Test* counts only the rounds of the role itself (10). The other tests ran with me on the
real devices and are listed there on their own line (6).

## Files

Both rounds were written in German, the working language of the whole system. The English versions were translated
afterwards for readers; the German files are the originals.

| File | Language | Translation |
|---|---|---|
| [`HANDOFF-bau-73.md`](HANDOFF-bau-73.md) | German (original) | [`HANDOFF-bau-73.en.md`](HANDOFF-bau-73.en.md) |
| [`ABSCHLUSS-bau-73.md`](ABSCHLUSS-bau-73.md) | German (original) | [`ABSCHLUSS-bau-73.en.md`](ABSCHLUSS-bau-73.en.md) |
| [`HANDOFF-design-52.md`](HANDOFF-design-52.md) | German (original) | [`HANDOFF-design-52.en.md`](HANDOFF-design-52.en.md) |
| [`ABSCHLUSS-design-52.md`](ABSCHLUSS-design-52.md) | German (original) | [`ABSCHLUSS-design-52.en.md`](ABSCHLUSS-design-52.en.md) |

**Redacted:** wherever a text named my domain, an address in my home network or a server provider, it now says
`[entfernt]` ("removed"). In Design 52 that is only the list of search words the role used to check that nothing
private was published. Build 73 needed no redaction. Everything else is as written, including file paths on my PC
and my first name.
