# Facilitator Guide — VM Test Run (Lite Edition)

This file is for facilitators, not participants. It has three parts:

1. **[Prepare](#1-prepare-human-5-minutes)**: what a human does once.
2. **[Agent test plan](#2-agent-test-plan)**: hand this to Claude Code in a workspace. It tests the lab and writes a report.
3. **[Human-only checks](#3-human-only-checks-10-minutes)**: things only a person in a browser can check.

Section [4](#4-workspace-image-checklist) is the checklist for whoever builds the workspace image.

---

## 1. Prepare (human, 5 minutes)

1. Open a fresh participant workspace, exactly as a participant would get it.
2. Open a terminal and start Claude in the lab folder:

   ```bash
   cd ~/workshop-agent-lab   # or wherever the lab is cloned
   claude
   ```

   Accept folder trust. Switch to **Accept edits** mode (`Shift+Tab`) so the agent can write its report.

3. Paste:

   ```text
   Read FACILITATOR.md and run section 2, the agent test plan, completely.
   Ask me only when a step is marked HUMAN or when you are blocked.
   ```

The run takes about 15–25 minutes. Approve the shell commands it asks for (`claude -p`, `cp`, `python3`, `curl`).

---

## 2. Agent Test Plan

> **Instructions for the agent running this plan.**
>
> You are testing a beginner workshop before real participants use it. Participants are new to AI agents and many
> are not developers. Find everything that would confuse or block them.
>
> Rules:
>
> - Do **not** edit anything in this folder except `outputs/vm-test-report.md`.
>   All participant-style work happens in a **copy** at `/tmp/lab-test` (created in T0).
> - Never use or invent real company data.
> - Run every check, even if an earlier one failed. Mark each `PASS`, `FAIL`, `WARN`, or `BLOCKED` with a one-line reason.
>   Paste the exact error message for every `FAIL`.
> - For each problem, propose a concrete fix: README step plus replacement text.
> - Write the report to `outputs/vm-test-report.md` (format at the end). Update it after each check.
>
> **How to simulate a participant.** Start fresh Claude Code sessions in print mode from inside the test copy:
>
> ```bash
> cd /tmp/lab-test && claude -p "<the prompt a participant would type>" --permission-mode acceptEdits < /dev/null
> ```
>
> `acceptEdits` stands in for a participant clicking **Yes** on file changes. It loads the copy's `CLAUDE.md`,
> skills, and helper agents. After each prompt, look at the files it created, not just its answer.

### T0. Environment

```bash
claude --version
git --version
python3 --version
echo $HOME; pwd; ls -la
```

- `PASS` if all work. Compare the lab path with what a participant sees (README step 1.1 expects `README.md`, `CLAUDE.md`,
  `backlog`, `examples` in the file list).

Create the test copy:

```bash
rm -rf /tmp/lab-test && cp -r "$(pwd)" /tmp/lab-test && rm -rf /tmp/lab-test/outputs/* && find /tmp/lab-test/backlog -type f ! -name .gitkeep -delete
```

### T1. Model access

```bash
cd /tmp/lab-test
claude -p "Reply with exactly READY" < /dev/null
```

`FAIL` if it errors.

### T2. Step 1: meet Claude

Run every prompt from README steps 1.3 and 1.4 in order. Check:

- The answers are understandable for a beginner (short, no jargon).
- `outputs/hello.md` exists after 1.4.

### T3. Step 2: real work without a skill

Run the prompts from README 2.1, 2.2, and 2.3 in order (2.3: clear the backlog, then run the 2.1 prompt again).

- `PASS` if `backlog/` contains ticket files after 2.1 and again after 2.3.
- Compare the two runs: do the ticket structures differ? The README claims they probably will. If they are nearly
  identical (because `CLAUDE.md` already describes the ticket format), report `WARN`: the "aha" moment before step 3
  would fall flat. Propose a fix (for example removing the ticket format from `CLAUDE.md`).

### T4. Step 3: skills

1. Run `/poem-writer a meeting that could have been an email` (README 3.2). `PASS` if it writes a poem.
2. Create `.claude/skills/ticket-writer/SKILL.md` exactly as README 3.3 shows (copy it; do not improve it).
3. Clear the backlog and run `/ticket-writer examples/meeting-notes.md`, then `/ticket-writer examples/bug-reports.md`.
   - `PASS` if every ticket has: title starting with a verb, Context, User story, Acceptance criteria (Given/when/then),
     Open questions, Labels from feature/bug/chore only.
4. Run the "Make it yours" prompt from README 3.5 and check that the skill file changed accordingly.

### T5. Step 4: helper agent

1. Create `.claude/agents/backlog-reviewer.md` exactly as README 4.2 shows.
2. Clear the backlog, then run the 4.3 prompts in order:
   copy the sample tickets, then *"Use the backlog-reviewer agent to review our backlog."*
   - `PASS` if the answer is a table with all three tickets, and `002-fix-booking` and `003-dark-mode` are **not** rated "ready".
3. Run *"Ask the backlog-reviewer to fix the worst ticket."* and check the ticket files.
   - `PASS` if no ticket file changed **and** the answer explains the reviewer can only give feedback.
   - `WARN` if the main session fixed the ticket itself instead (participants might find that confusing). Note what happened.

### T6. Step 5: put it together

Run the 5 prompt (ticket-writer → backlog-reviewer → improve). `PASS` if new tickets appear, a review is shown, and tickets
not rated "ready" were changed.

Optional page: run the "make a page out of it" prompt. Check that `outputs/backlog.html` exists and that a server was
started on a port between 3000 and 9000, bound to `0.0.0.0`. Check with `curl`. Leave it running and add **HUMAN-T6**
with the port to the report.

### T7. Helper skills

```bash
cd /tmp/lab-test
claude -p "/skill-helper review the ticket-writer skill" < /dev/null
claude -p "/use-case-coach" < /dev/null
claude -p "/use-case-coach Here is a real ticket from our internal system ACME-CRM: customer Max Mustermann, contract 4711, cannot log in." < /dev/null
```

- `PASS` if `skill-helper` gives concrete, beginner-friendly review points and does not edit files without asking.
- `PASS` if `use-case-coach` asks **one** first question.
- `PASS` if the third prompt pushes back on the internal-looking data.

### T8. Step 6: own idea

Act as a participant who works as a Scrum Master. Run README 6.3 with the sentence
*"When a sprint ends, the helper takes the list of finished tickets and creates release notes for our users."*
Then run 6.4 with `/skill-helper` (answer its questions yourself in follow-up `claude -p --continue` calls),
and run the resulting skill on `examples/my-data/`.

- `PASS` if invented examples, a working skill, and a result exist at the end.
- Note how many back-and-forth turns it took. More than six is a `WARN` for a one-hour slot.

### T9. Read-through for beginners

Read `README.md` from top to bottom as someone who has never used an AI agent or a terminal. List:

- every term or action used before it is explained
- every step without a ✅ check
- every place where the README and what you saw in T2–T8 disagree (file names, messages, results)
- every step that is likely to take much longer than the timing table says

Keep this to the 10 most important findings, ranked.

### T10. Cleanup

Stop any web servers you started. Leave `/tmp/lab-test` for the facilitator. Do not commit anything.

### Report format

Write `outputs/vm-test-report.md`:

```markdown
# VM Test Report (Lite) — <date>

## Summary
- Ready for participants: yes / yes with fixes / no
- Fails: <count>  Warnings: <count>
- HUMAN checks waiting: <list with ports>

## Results
| Check | Result | Note |
| --- | --- | --- |
| T0 Environment | PASS | ... |

## Fixes needed before the workshop (ranked)
1. <problem> — <README step> — <proposed fix, exact text>

## Read-through findings (T9)
1. ...
```

---

## 3. Human-Only Checks (10 minutes)

In a fresh participant workspace:

- [ ] **First start**: `claude` asks for the theme and folder trust only. No login screen.
- [ ] **Permission box** appears for README 1.4 with Yes / Yes, and don't ask again / No.
- [ ] **Paste** in the terminal works with `Ctrl+Shift+V` (Mac: `Cmd+V`) and right-click → Paste.
- [ ] **Markdown preview** of `README.md` works (right-click tab → Open Preview).
- [ ] **Restart** with `/exit` and `claude` works, and a new helper agent is picked up afterwards.
- [ ] **HUMAN-T6**: the web page opens from the **Ports** tab or the notification.
- [ ] **Persistence**: close the tab, reopen the link later. Files are still there.
- [ ] **Timing**: steps 1–2 should take about 45 minutes for a beginner. Note your time and add 50% for participants.

---

## 4. Workspace Image Checklist

- [ ] Repo cloned into the home folder (e.g. `~/workshop-agent-lab`), and new terminals open in that folder.
- [ ] `claude` installed, on the `PATH`, and already authenticated (participants never see a login screen).
- [ ] The model configured for participants works for all of them at the same time (rate limits).
- [ ] `python3` available (web page in step 5).
- [ ] Port forwarding works for the optional web page (Coder **Ports** tab).
- [ ] No other AI assistant extension active in VS Code, or participants are told to ignore it.
- [ ] Workspaces are wiped after the workshop.
