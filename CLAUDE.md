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
