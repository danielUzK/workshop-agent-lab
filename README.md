# Agent Lab — Your First Steps With An AI Agent

In this lab you will work with **Claude Code**, an AI assistant that does not just chat:
it can read files, write files, and carry out tasks for you.

You will learn three things, one after the other:

1. **Talk to an agent** and let it do real work.
2. **Write a skill**: teach the agent *how* you want a task done.
3. **Create a helper agent**: give one task to a specialist with a clear job.

Then you use what you learned for an idea from **your own work**.

The example throughout the lab is everyday work in a development team:
writing tickets, checking a backlog, preparing a standup.
You do not need to be a developer. If you can write an email, you can do this lab.

> [!CAUTION]
> **Data rule.** Never type, paste, or upload company-internal information in this lab:
> no real tickets, documents, code, customer or colleague names, system names, or screenshots from work.
> Use the fictional examples in this lab, or invent your own.
> If you are unsure whether something counts as internal: it does. Leave it out.

## Before You Start

**You need:** a laptop with a browser, and the link to your personal workspace (the facilitators hand these out).

**How this guide works:**

- Text in a grey box like this is something you **type to Claude** (or copy and paste):

  ```text
  Hi, what is in this folder?
  ```

- ✅ lines tell you what you should see when a step worked.
- 💡 lines are tips. You can skip them.
- Work in pairs if you like. One person types, one person reads along. Swap after each step.

**Timing:**

| Time | Step |
| --- | --- |
| 0:00-0:15 | 1. Open your workspace and meet Claude |
| 0:15-0:45 | 2. Let Claude do real work |
| 0:45-1:30 | 3. Your first skill |
| 1:30-2:00 | 4. Your first helper agent |
| 2:00-2:15 | 5. Put it together |
| 2:15-end | 6. Your own idea, then a short demo |

## 1. Open Your Workspace And Meet Claude

### 1.1 Open the workspace

Open the workspace link in your browser and sign in.
You see an editor (it is called VS Code) with a list of files on the left.

✅ On the left you see `README.md`, `CLAUDE.md`, `backlog`, and `examples`.

Your workspace is personal and keeps your files, even if you close the tab. Just open the link again.

💡 To read this guide nicely formatted inside the workspace: click `README.md`, then right-click its tab and choose **Open Preview**.

### 1.2 Open the terminal and start Claude

A **terminal** is a text window where you type commands. You only need one command in this whole lab.

1. In the menu (☰ top left), choose **Terminal → New Terminal**. A panel opens at the bottom.
2. Type `claude` and press Enter.
3. Claude may ask a few questions the first time:
   - **Theme**: pick any.
   - **Do you trust this folder?** Choose **Yes**.

✅ You see a box at the bottom of the terminal where you can type.

💡 Copy and paste in the terminal: use `Ctrl+Shift+V` (Mac: `Cmd+V`) or right-click → **Paste**.
If the browser asks whether the page may use the clipboard, choose **Allow**.

### 1.3 Say hello

Type:

```text
Hi! What is in this folder? Explain it to me in three sentences.
```

✅ Claude answers and mentions this guide and the examples.

Try a few more questions. Ask anything:

```text
What can you do in this workspace? Give me five examples.
```

```text
Explain in simple words what a "user story" is.
```

### 1.4 Claude asks before it acts

Ask Claude to create a file:

```text
Create a file outputs/hello.md with a short welcome message for our team.
```

Before Claude changes anything, it shows a box asking **"Do you want to ...?"** with a few options:

- **Yes**: allow it this one time.
- **Yes, and don't ask again**: allow it for the rest of the session.
- **No** (or `Esc`): don't do it. Then tell Claude what you want instead.

Choose **Yes**.

✅ A new file `outputs/hello.md` appears on the left. Click it to see what Claude wrote.

This is the most important habit with agents: **read what it wants to do, then decide.**
The agent does the work, you stay in charge.

## 2. Let Claude Do Real Work

The folder `examples/` contains fictional material for a fictional app called **RoomBuddy**,
a meeting-room booking app. Have a look at `examples/meeting-notes.md`: messy notes from a planning meeting.

### 2.1 From notes to tickets

```text
Read examples/meeting-notes.md and turn it into tickets.
Save each ticket as its own file in the backlog folder.
```

✅ New files appear in `backlog/`. Open two of them.

Discuss with your neighbour:

- Are these tickets good? Would your team accept them?
- What is missing? What would you do differently?

### 2.2 Ask for changes

Agents get better when you tell them what you want. Try:

```text
The tickets are too long. Make each one shorter and add acceptance criteria
in the form "Given ... when ... then ...".
```

### 2.3 Try it again from scratch

Clear the backlog and run the same request again:

```text
Delete all files in the backlog folder except .gitkeep.
```

```text
Read examples/meeting-notes.md and turn it into tickets.
Save each ticket as its own file in the backlog folder.
```

Compare: are the new tickets the same as before?

Probably not. They have a different structure, different sections, maybe different labels.
Every time, you would have to explain again what a good ticket looks like for your team.

**That is exactly the problem a skill solves.**

## 3. Your First Skill

### 3.1 What is a skill?

A **skill** is a short text file with instructions for one task.
Think of it as a **recipe** or a **checklist** that you give to a new colleague:
"This is how we write tickets in our team."

Once a skill exists, Claude follows it every time. You do not have to explain it again.

A skill is just a text file in a special folder:

```text
.claude/skills/<name-of-the-skill>/SKILL.md
```

### 3.2 Look at an example

This lab comes with one small skill, `poem-writer`. Open it in the file list:
`.claude` → `skills` → `poem-writer` → `SKILL.md`.

It has two parts:

1. **The top part** between the `---` lines: a **name** and a **description**.
   The description tells Claude *when* to use the skill.
2. **The rest**: the instructions. Plain sentences and bullet points.

Try it:

```text
/poem-writer a meeting that could have been an email
```

✅ Claude writes a short poem.

Typing `/` plus the skill name starts a skill directly.
You can also just ask *"Write me a short poem about Mondays"*: Claude notices that the request
matches the skill's description and uses it by itself.

### 3.3 Write your own skill: ticket-writer

Now you write a skill that describes **how your team wants tickets to look**.

Copy the text below, then tell Claude:

```text
Create the file .claude/skills/ticket-writer/SKILL.md with exactly this content:
```

and paste the text after it:

```text
---
name: ticket-writer
description: Use this skill to turn rough notes, a bug report, or an idea into a ticket for the backlog.
---

# Ticket Writer

Turn the input into one or more tickets. Save each ticket as a file in backlog/.

Every ticket has:

- a short title that starts with a verb
- Context: why this matters, in 1-2 sentences
- User story: "As a <role>, I want <goal>, so that <benefit>."
- Acceptance criteria: 2-4 items in the form "Given ... when ... then ..."
- Open questions: things that are unclear (never invent answers)
- Labels: only feature, bug, or chore

Keep each ticket short enough to read in one minute.
```

✅ The file appears under `.claude/skills/ticket-writer/`.

### 3.4 Use your skill

Clear the backlog first:

```text
Delete all files in the backlog folder except .gitkeep.
```

Then:

```text
/ticket-writer examples/meeting-notes.md
```

✅ The new tickets in `backlog/` all follow the same structure.

Run it a second time on `examples/bug-reports.md`:

```text
/ticket-writer examples/bug-reports.md
```

Same structure again. That is the power of a skill: **consistent results, every time.**

### 3.5 Make it yours

The skill above is a starting point. Change it so it matches what **you** think a good ticket is.
You can ask Claude to edit it:

```text
Change the ticket-writer skill: every ticket should also have a "Who is affected" line,
and the acceptance criteria should include at least one case where something goes wrong.
```

Then run it again and check the result.

💡 Ideas for rules: maximum length, a priority, a "definition of done", tone of voice, a language (English or German).

### 3.6 What makes a good skill?

| Good skills are... | For example |
| --- | --- |
| **about one task** | "write tickets", not "do everything with tickets" |
| **clear about when to use them** | the description names the task and typical requests |
| **concrete** | "2-4 acceptance criteria" instead of "good acceptance criteria" |
| **honest about gaps** | "list open questions, never invent answers" |

## 4. Your First Helper Agent

### 4.1 What is a helper agent?

A **helper agent** (Claude calls it a *subagent*) is a specialist with **one job**.
Think of it as a colleague with a job title, for example "backlog reviewer".

How is that different from a skill?

| | Skill | Helper agent |
| --- | --- | --- |
| Is like... | a recipe | a colleague with a job title |
| Answers... | *how* is this task done? | *who* does this task? |
| Special power | consistent results | works on its own, with only the tools you give it |

Two reasons to use a helper agent:

- **Focus**: it only does its one job and reports back a short result.
- **Limits**: you decide what it is allowed to do, for example *read files but never change them*.

### 4.2 Create a backlog reviewer

This helper checks tickets and gives feedback, like a strict product owner.
It is only allowed to **read**, so it cannot change your tickets.

Tell Claude:

```text
Create the file .claude/agents/backlog-reviewer.md with exactly this content:
```

and paste:

```text
---
name: backlog-reviewer
description: Use this agent to review the tickets in the backlog folder and say which ones are ready to work on.
tools: Read, Grep, Glob
---

You are an experienced, friendly product owner.

Read every ticket in the backlog folder. For each ticket, check:

- Is the goal clear?
- Can the acceptance criteria be tested?
- Is it small enough for a few days of work?

Rate each ticket: ready, almost ready, or needs work.

Return a table with: ticket, rating, and the one most important improvement.
You cannot change files. Only give feedback.
```

The line `tools: Read, Grep, Glob` means: this agent may only **read and search** files. Nothing else.

**Important:** Claude only notices new helper agents when it starts. Restart Claude:
type `/exit`, press Enter, then type `claude` and press Enter again.

### 4.3 Use it

The `examples/sample-tickets/` folder has three tickets: one good, two not so good.
Copy them into the backlog so the reviewer has something to work with:

```text
Copy all files from examples/sample-tickets into the backlog folder.
```

Then:

```text
Use the backlog-reviewer agent to review our backlog.
```

✅ You get a table with a rating for each ticket. "fix booking" and "Add dark mode, CSV export, ..." should not be rated "ready".

Now test its limits:

```text
Ask the backlog-reviewer to fix the worst ticket.
```

✅ Claude explains that the reviewer can only give feedback: it has no tool to change files, so it really cannot.
Claude itself, however, *can* change files. It will usually offer to fix the ticket, or ask you before doing it.

That is the point of a helper agent: **you decide what each helper is allowed to do.**
The reviewer only judges. The writing stays with Claude, and with you approving.

### 4.4 Make it yours

Change one thing, then restart Claude and run it again:

- a different personality: *"a strict tester"*, *"a new developer on the team"*
- a different check: *"Does every ticket say which user group is affected?"*
- a different output: *"Sort the table from worst to best"*

## 5. Put It Together

Now your skill and your agent work as a team:

```text
Use the ticket-writer skill to turn examples/bug-reports.md into tickets in the backlog folder.
Then use the backlog-reviewer agent to review them.
Finally, improve every ticket that is not "ready", based on the feedback.
```

Watch what happens: Claude writes, the reviewer checks, Claude improves.
That is an **agentic workflow**: several steps, different roles, and you approving along the way.

### Optional: make a page out of it

```text
Create outputs/backlog.html: a simple, nice-looking web page that shows all tickets
in the backlog folder as cards, grouped by label. Then start a web server so I can open it.
```

To open it: in the terminal panel, click the **Ports** tab and then the globe icon next to the port,
or click **Open in Browser** in the notification that appears.

💡 Typing `localhost` in your own browser does not work: the workspace runs in the cloud, not on your laptop.

### Optional: one more skill

Ideas you can build in 10 minutes:

- `standup-writer`: turn a few bullet points into a three-line standup update (yesterday, today, blockers).
- `release-notes`: turn a list of finished tickets into notes for users, not developers.
- `test-case-writer`: turn acceptance criteria into a list of test cases, including edge cases.

💡 Use the skill helper: `/skill-helper I want a skill that turns acceptance criteria into test cases`.
It asks you a few questions and writes the skill with you.

## 6. Your Own Idea

Now use what you learned for something from **your own work**.
The goal is a small, working skill (and maybe a helper agent) for one real task.

> [!CAUTION]
> The data rule matters most now. Describe your task, never paste real content.
> Let Claude create **invented** example material instead (see 6.3).

### 6.1 Find an idea (10 minutes)

Each person writes down answers to these questions, then share them with your neighbour:

- Which task did you do **several times** last month, almost the same way?
- Which text do you write that always **follows the same pattern**? (tickets, emails, reports, minutes, reviews)
- What do you **check** before something is "done"?
- What do **new colleagues** always ask you?

Good first ideas are small and boring. That is a feature, not a bug.

| Type of helper | What it does | Example |
| --- | --- | --- |
| **Writer** | turns rough input into a structured text | meeting minutes from notes, status email from bullet points |
| **Checker** | checks something against your rules | "is this ticket ready?", "does this document follow our template?" |
| **Converter** | turns one format into another | acceptance criteria → test cases, notes → checklist |
| **Explainer** | explains something in simple words | "explain this error message", "summarize this long text for a manager" |
| **Coach** | asks you questions and guides you | retrospective questions, preparing a difficult conversation |

**No idea yet?** Let Claude interview you:

```text
/use-case-coach
```

💡 Want to write it down properly? Ask Claude: *"Copy templates/use-case-canvas.md to outputs/my-idea.md and help me fill it in, one question at a time."*

### 6.2 Describe it in one sentence

Fill in:

> When **[situation]**, the helper takes **[input]** and creates **[output]** for **[who]**.

Example: *When a sprint ends, the helper takes the list of finished tickets and creates release notes for our users.*

Then write down three rules for a **good** result, and one thing that must **never** happen.

### 6.3 Create invented example material

```text
I want to build a helper that <your sentence>.
Create 5 invented examples of the input in examples/my-data/.
Make them realistic: messy, incomplete, with typical mistakes. Invent all names and details.
```

Check them: do they feel like the real thing? If not, tell Claude what is missing.

### 6.4 Build it

Write your skill with the skill helper:

```text
/skill-helper <your sentence> — the rules for a good result are: ...
```

Then try it on your examples:

```text
/<your-skill-name> examples/my-data/
```

Improve it until you are happy. Add a helper agent only if you need a separate reviewer or want to limit what it can do.

### 6.5 Show it (3 minutes per team)

1. **The problem**: who has it, and how often?
2. **Live demo**: run your skill on one example.
3. **What you learned**: what worked, what surprised you?
4. **To use it for real**: what would you need? (data, approval, tools)

💡 Let Claude prepare your talking points: *"Write a 3-minute demo script about the skill I built."*

## Quick Help

**Commands you can type in Claude:**

| Type | What it does |
| --- | --- |
| `/skills` | show all skills |
| `/clear` | start a fresh conversation |
| `/exit` | quit Claude (type `claude` to start again) |
| `Esc` | stop Claude or close a menu |

**If something does not work:**

| Problem | Try this |
| --- | --- |
| Claude does not react | Press `Esc`, then type your request again. |
| My new skill does not show up | Check the file path: `.claude/skills/<name>/SKILL.md`. Then restart Claude. |
| My new helper agent is not used | Restart Claude (`/exit`, then `claude`). Then name it in your request: *"Use the ... agent"*. |
| The result is not what I wanted | Tell Claude exactly what is wrong. Then put that rule into your skill. |
| I am lost | Ask Claude: *"I am doing step 3.4 of the README. What should I do next?"* |

**Words in this lab:**

| Word | Meaning |
| --- | --- |
| **Agent** | An AI that can take actions (read and write files, run tasks), not just chat. |
| **Prompt** | What you type to the agent. |
| **Skill** | A text file with instructions for one task. Like a recipe. |
| **Helper agent** (subagent) | A specialist with one job and limited tools. Like a colleague with a job title. |
| **Tools** | The actions an agent can take, for example reading or writing files. |
| **CLAUDE.md** | A file with general rules for this folder. Claude reads it every time it starts. |

## What You Take Home

- An agent is not magic. It follows instructions, and **better instructions give better results**.
- **Skills** turn your know-how into instructions the agent follows every time.
- **Helper agents** give a task a clear owner and clear limits.
- **You stay in charge**: read what the agent wants to do, then decide.

Want to keep your skills and helper agents? Before the end of the workshop, right-click the `.claude` folder
in the file list → **Download**. Your workspace is deleted after the workshop.

## License

This workshop material is released under the MIT License. See [LICENSE](LICENSE).
