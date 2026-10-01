# Use-Case Canvas

> Describe the problem, never the real data. Use invented examples only.

## 1. The problem

- **Who has it?** (role, not name)
- **How often?** (per day / week / sprint)
- **What do they do today, step by step?**
- **What is annoying, slow, or error-prone about it?**

## 2. The agent in one sentence

> When <trigger>, the agent <does what> using <which input>, and returns <which output> to <whom>.

## 3. Pattern

Drafter / Checker / Converter / Triage / Reporter / Coach / Pipeline (see README, Part 2)

## 4. Input and output

| | Description | Invented example in this repo |
| --- | --- | --- |
| Input | | `examples/my-data/...` |
| Output | | |

What does a **good** output look like? Write down 3 concrete rules.

1.
2.
3.

## 5. Building blocks

| Block | Name | Responsibility |
| --- | --- | --- |
| Skill | | |
| Subagent (optional) | | |
| MCP server (optional) | | |

## 6. Guardrails

| Action | allow / ask / deny | Why |
| --- | --- | --- |
| | | |

- Where must a human approve?
- What must **never** happen?
- What could a prompt injection in the input try to do?

## 7. To make it real

- Which real data or system would it need?
- Who would have to agree (system owner, data protection, team)?
- What is the smallest real pilot? (read-only, one team, two weeks)
- How would you know it helps? (time saved, fewer review rounds, fewer incomplete tickets)
