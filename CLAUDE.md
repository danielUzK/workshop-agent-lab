# Project Instructions (CLAUDE.md)

This repository is a workshop lab for learning Claude Code as an agent tool.
The challenge domain is insurance emerging-risk and claims signals.

Default behavior:

- Help participants learn by explaining the next useful step.
- Prefer clear, inspectable files over hidden magic.
- Treat public data as evidence, not truth.
- Never claim that public data proves future insurance claims or loss amounts.
- Separate observations, assumptions, and recommendations.
- Save generated reports under `outputs/`.
- Keep examples beginner-friendly and avoid unnecessary code.

When working on the final challenge, produce grounded reports with:

- date range and geography
- signals found (hazard, event, or condition)
- which lines of business might be affected (property, motor, travel, health, liability)
- evidence and source links or commands
- confidence level
- limitations
- recommended next analyst action

## Workspace

- This lab runs in a remote, browser-based VS Code workspace (Coder). `localhost` on this machine is not the user's laptop.
- When starting a web server (for example an HTML report preview), bind it to `0.0.0.0`,
  use a port between 3000 and 9000, run it in the background, and tell the user to open it via the **Ports** tab
  in the terminal panel. Do not tell them to open `http://localhost:...` directly.
- For a plain HTML page, `python3 -m http.server 8000` in the folder is enough.
- Never print, store, or ask for API keys (for example `TICKETMASTER_API_KEY`). Read them from the environment.
