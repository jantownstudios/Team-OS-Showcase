<!-- NAME UND KONTAKT: Jan entscheidet -->

**English** · [Deutsch](README.de.md)

# Team-OS: an operating system for an AI team

I run several AI sessions like a small team: a **multi-agent system**. For each project I plan with one lead agent,
the **Master**. Specialised agents, the **roles**, work in the background from written task briefs. A Windows app I
had built for this orchestrates them all and enforces the rules.

This repo is a **showcase**: it shows what was built and how the work is done. The code itself is private. A real,
checkable example of one round of work is in [`beispiel/`](beispiel/).

![Architecture: I plan with the Master, the Master briefs the agent roles, the app starts them, provides MCP tools and checks every tool call with hooks, every session runs on Claude Code](bilder/architecture.png)

## At a glance

| | |
|---|---|
| **What** | a fixed way of building software with AI agents, plus a desktop app of my own that orchestrates and guards it |
| **Who** | me, as <!-- titel -->AI Agent Builder<!-- /titel -->, with many projects at once |
| **AI** | Claude Code (Anthropic's official CLI), run headless, on my own subscription |
| **App** | Windows, written in Rust, UI with egui; plus a web app for phone and laptop |
| **Period** | app since 2026-10-27, this page as of 2026-11-10 |
| **My role** | designed the system, made the decisions, signed off every delivery. **The code was written by the AI roles.** |

## What it is

An AI session forgets everything once it ends. And it does not reliably check its own work. Team-OS solves both with
clear responsibilities and with files as memory.

| Term | What it is |
|---|---|
| **Master** | the lead agent of a project (orchestrator). Plans with me, writes task briefs, reviews results. Builds nothing itself, so there is always a second pair of eyes. |
| **Role** | an agent session for exactly one task; then it ends. The app project has five: **Build** (writes code), **Design** (look and feel), **Test** (tries out third-party tools), **Red Team** (tries to get past the app's safeguards), **Research** (reads sources). Their working names are German: Bau, Design, Test, Gegenprobe, Recherche. |
| **Handoff** | the written task brief for a role: goal, my decisions, which files it may change and which not. |
| **Completion report** | the role's report back (*Abschluss*): what changed, how it checked it, what is still open. |
| **Core** | a small set of rules that every Master follows. Each rule names the incident it came from. |
| **App** | starts and supervises all sessions and enforces the rules technically. |

Each role works in its own copy of the project (Git worktree). So several roles can work in parallel without getting
in each other's way.

## My role, honestly

| Me | The AI roles |
|---|---|
| designed the system: rules, roles, workflows, review stages | write the code, the tests and the design |
| decide what gets built and in which order | carry out exactly the brief the Master writes |
| answer approvals (for example: may this role start?) | ask when a safeguard stops them |
| sign off every visible change in the real window | report in their completion report what they checked themselves |

I do not program the app line by line. My work is the **system behind it**: how tasks are cut, who may do what, how
things are checked, and when something is done.

## How the work is done

| Step | Who | What happens |
|---|---|---|
| 1 · Plan | me and Master | We decide what comes next. The decision is written to a file with its date. |
| 2 · Brief | Master | writes the handoff for a role. |
| 3 · Start | me | One click on "Allow" starts the role. Without the click nothing starts. |
| 4 · Delivery | role | works in the background and writes a completion report. |
| 5 · Review | Master | does not trust the report but checks itself: tests, reading the changes, its own screenshots. |
| 6 · Red team | role Red Team | on security topics, deliberately tries to get past the safeguards. If it finds a way, it goes back to Build. |
| 7 · Sign-off | me | look at the result in the real window. |
| 8 · Release | Master | only after my go: new version, installer, notes for all Masters. |

**Why so many stages?** A report is not a review, and a review is not a sign-off. Each stage catches different
mistakes. "All tests green" alone does not count as checked here. These rules come from real incidents in earlier
projects.

**See for yourself:** [`beispiel/`](beispiel/) holds the real handoff and completion report of the round that built
the first version of this showcase.

## What the app does

A selection. The technical term is in brackets.

| Feature | In short |
|---|---|
| **Start and supervise roles** (orchestration, MCP tools `rolle_starten`, `rolle_abbrechen`) | The Master starts roles itself through a tool of the app; I confirm with a click. A star per role shows who is working. I can stop any role. |
| **Own tools for the agents** (custom MCP server) | The app brings its own MCP server. Masters and roles call its tools: start or stop a role, message another project, read their inbox, report a bug or wish (`wunsch_melden`), use the browser. |
| **Approvals with one click** (human in the loop, permission prompts as pop-ups) | Sensitive steps (start a role, delete, push) wait for my "Allow", on the PC, the phone or the laptop. |
| **A check on every tool call** (hooks `PreToolUse`, `Stop`) | Before any tool runs, a hook asks the app: is this tool switched off, is the address on the block list, does this file need my approval? If the hook hangs or crashes, the tool is blocked (fail closed). A `Stop` hook sends a fresh Master back if it wants to answer before it has read its required files. |
| **Dashboard** (tool permissions per project) | I switch off single tools of the AI, for all projects or just one. A switched-off tool does not exist for the AI. |
| **Own browser** (browser automation via MCP tools `browser_oeffnen`, `browser_lesen`, `browser_klicken` …, URL block list) | The AI browses in a browser inside the app, with a block list for addresses. Before it submits a form, the app asks me. |
| **Messages between Masters** (agent-to-agent messaging, MCP tools `funk_senden`, `post_lesen`) | A Master can send another project a task. I see every message word for word before it goes out. |
| **Master handover** (context management) | When a Master's memory fills up, it saves its state to a file and a fresh Master takes over. |
| **Phone and laptop** (web app, web push) | Projects, approvals and chat on the go; the Team Center shows all running Masters and roles on one page. |

### Screenshots

All screenshots come from the app's automated checks and show **made-up test data**. The app itself is in German.

**Master page:** navigation and inbox on the left, the chat with the Master in the middle, the roles on the right.
Here the app asks how the role "Bau" (Build) should stop.

![Master page with a card for stopping a role](bilder/01-master-seite.png)

**Dashboard:** switch the AI's tools on and off, block list for addresses.

![Dashboard with switches for tools and a block list](bilder/02-dashboard.png)

**Team Center on the laptop:** all Masters, running roles and messages between projects on one page.

![Team Center with Masters, roles, limits and inbox](bilder/03-team-center.png)

**Roadmap on the laptop:** a project's plan, with the role that is working on a step right now.

![Roadmap of a project with running and planned steps](bilder/04-roadmap.png)

**Approval on the phone:** a role wants to run a command and waits for "Allow" or "Deny".

<img src="bilder/05-freigabe-handy.png" alt="Approval card in the chat on the phone" width="320">

## Numbers

As of 2026-11-10. Counted from the Git history (versions, commits) and the numbered handoffs (rounds per role); tests
from the last full test run, projects from the app's project list.

| | |
|---|---|
| App started | 2026-10-27 |
| Versions | **29**, from 0.1.0 (2026-10-28) to 0.2.14 (2026-11-10) |
| Commits in the code | 491 |
| Rounds per role | Build 73 · Design 53 · Red Team 29 · Test 10 · Research 13 |
| Hands-on tests with me | 6, on the real phone, laptop and PC |
| Automated tests | 708 in the last full run |
| Projects in the app | about 29 |

Across the whole Team-OS, all projects together (as of 2026-10-31):

| | |
|---|---|
| Projects with their own code repo | 22 |
| Task briefs to roles (handoffs) | 532 |
| Decisions with date and reason | 351 |

## Technology

| | |
|---|---|
| Language | Rust |
| UI | egui (desktop), HTML/CSS/JavaScript (phone and laptop) |
| Agents | Claude Code started by the app, headless (`claude -p`, `stream-json`), one process per session; no login of its own, no API key |
| Tools | the app's own MCP server; hooks (`PreToolUse`, `Stop`) on every session |
| Collaboration | Git; each role in its own working copy (worktree) |
| Local | Everything runs on my PC; phone and laptop reach it through a private encrypted connection of my own. No cloud services, no telemetry. |

---

All rights reserved. Text and images for viewing only.
