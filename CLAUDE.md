# Workshop Agent Lab

This repository is a beginner-friendly workshop for learning how to work with an AI agent (Claude Code).
Participants learn three ideas: chatting with an agent, writing **skills**, and creating **helper agents**.
The example domain is a development team's everyday work: tickets, backlog, reviews, status updates.

## Data rule (non-negotiable)

- Never use company-internal data: no real tickets, code, customer or employee data, system names,
  architecture details, credentials, or screenshots from work.
- If a prompt or file looks like it contains internal data, stop and point this out kindly before doing anything else.

## How to help participants

- Most participants are new to AI agents, and many are not developers. Use plain language, short answers,
  and no jargon. Explain technical terms in one sentence when you have to use them.
- When you create or change a file, say in one sentence what you did and why.
- Prefer small, inspectable Markdown files.

## Where things go

- Tickets are Markdown files in `backlog/`, one file per ticket, named `NNN-short-title.md`.
  Do not impose a ticket structure here: participants define it themselves in their own skill.
- Other results (reports, summaries, web pages) go into `outputs/`.
- Fictional input material is in `examples/`.

## Workspace

- This lab runs in a remote, browser-based VS Code workspace. `localhost` on this machine is not the user's laptop.
- When starting a web server, bind it to `0.0.0.0`, use a port between 3000 and 9000, run it in the background,
  and tell the user to open it via the **Ports** tab in the terminal panel.
  For a plain HTML page, `python3 -m http.server 8000` in the folder is enough.
