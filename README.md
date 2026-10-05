# Workshop Agent Lab — Emerging Risk & Claims Signals

Welcome. In this lab you will build an agentic system with Claude Code.

The final goal is a grounded report about upcoming public events and conditions
in Europe that could create **unusual insurance claim activity**.
The system should find signals, evaluate them, and explain what a human analyst
in claims, risk, or underwriting should look at next.

You will not start by writing an agent framework. You will build with Claude Code-native building blocks:

- **skills**: reusable instructions for a capability
- **subagents**: specialist roles with their own instructions, context window, and tool access
- **the main Claude Code session**: the coordinator that plans, delegates, and combines results
- **public data sources**: evidence for the report

The README is the main guide. Move through it at your own pace.

For this version of the lab, use only live public sources and public APIs.
Do not use company-internal data.
Do not rely on prebuilt demo data unless a facilitator explicitly adds it later.

## Start Here

You need:

- your laptop with a browser (nothing to install)
- the link to your personal workspace (facilitators hand these out)

Every participant works in their **own workspace**: VS Code running in your browser,
on a machine in the cloud. Claude Code and this lab are already installed and logged in there.
The hands-on part starts at [step 0](#0-open-your-workspace).

> [!TIP]
> **You never have to edit a file by hand.** Whenever this guide says "create a file"
> or "change a setting", you can just ask Claude, for example:
> *"Create a skill called concise-summarizer in .claude/skills/."* Claude shows you the change and asks before saving it.
> Look out for the ✅ lines: they tell you what you should see when a step worked.

## Why This Case?

Insurers care about what is coming, not only what already happened.
A heatwave, a storm front, a public holiday travel peak, or a large stadium event
can each shift the pattern of claims that arrive in the following days:
property, motor, travel, health, liability, or event cancellation.

None of these public signals *prove* that claims will rise. They are early hints
that a human analyst can investigate with internal data. That gap — between a
public signal and a real claims forecast — is exactly what makes this a good
agent exercise: the system has to stay honest about what it does and does not know.

You are building an agentic system, not a prediction engine.

## What Is Claude Code?

Claude Code is Anthropic's agentic coding assistant that runs in your terminal
(it is also available in VS Code, JetBrains, a desktop app, and the browser).
Instead of only suggesting code inside an editor, it can work with the files in this folder and help execute a workflow.

In this lab, Claude Code can:

| Capability | What it means here |
| --- | --- |
| Chat | You give it natural-language instructions. |
| Read files | It can inspect this README, skills, agents, and outputs. |
| Write files | It can create skills, agents, summaries, and reports. |
| Run commands | It can call tools such as `curl` when you approve. |
| Search the web | It can use web search and web fetch to discover sources. |
| Use skills | It can load reusable instructions for specific tasks. |
| Use subagents | It can delegate work to specialist roles. |

Think of it as the **workspace where your agentic system runs**.
You will design the skills and agents; Claude Code will use them to do the work.

Important: Claude Code is powerful because it can read files, edit files, and run commands.
Review what it plans to do, especially before allowing terminal commands or using API keys.

## Which Permission Mode Should You Use?

In this lab, Claude Code has two jobs:

1. help you **build** skills and agents
2. help you **run** the agentic system you built

That can feel confusing at first, because both happen in the same terminal.
Use the mode based on what you are trying to do.
Press `Shift+Tab` to cycle through the modes. The status bar shows the active one.

| Mode | Status bar | Use it for |
| --- | --- | --- |
| **Manual** (`default`) | `⏸ manual mode on` | Default in this repo. Claude asks before edits and commands. |
| **Accept edits** | `⏵⏵ accept edits on` | Writing skills and agents quickly. File edits run without asking. |
| **Plan** | `⏸ plan mode on` | Let Claude explore and propose a plan before it changes anything. |
| **Auto** | `⏵⏵ auto mode on` | A safety classifier reviews actions instead of you. Only once your system is stable. |

This repo's `.claude/settings.json` starts every session in **Manual** mode.

Recommendation for this workshop:

- use **Manual** while running your agentic system, so you see every command and API call
- use **Accept edits** while writing skills and agents
- use **Plan** when designing something bigger

Never use `--dangerously-skip-permissions` in this lab.

## Which Model Should You Use?

Use a fast, low-cost model by default.
This lab is mostly about designing skills, agents, evidence trails, and reports.
You do not need the strongest model for every step.

| Task | Recommended model |
| --- | --- |
| Writing skills and agents | `haiku` or `sonnet` |
| Fetching and summarizing evidence | `haiku` |
| Reviewing weak evidence | `sonnet` or `opus` |
| Final report polish | `sonnet` or `opus` |

Switch models inside a session:

```text
/model
```

Subagents can have their own model. Add `model: haiku` (or `sonnet`, `opus`, `inherit`)
to an agent's frontmatter so cheap collection work runs on a cheap model
while the lead session uses a stronger one.

## Work In Balanced Teams

Form groups of around four people.

Aim for a mix of:

- people comfortable with terminals, files, or APIs
- people closer to claims, underwriting, risk, product, or process questions

This lab works best when one person can help with setup while others
challenge the business relevance, data quality, and report wording.
You are building an agentic system, not just a technical demo.

## License

This workshop material is released under the MIT License. See [LICENSE](LICENSE).

## How Technical Is This?

You do not need to be a developer to participate.

You will use a terminal in your browser workspace, but most steps can be done by asking Claude Code to create or edit files for you.
If terminal commands are unfamiliar, work in pairs and copy the commands exactly.

Use this rule of thumb:

- **Green path**: use Claude prompts, inspect what it created, and discuss the result.
- **Yellow path**: edit Markdown files by hand if you are comfortable.
- **Red path**: write code only if your team wants to go further.

In this lab, Markdown files are just text files.
Skills and agents are mostly structured text, not software engineering.

## Tiny Glossary

| Term | Short meaning |
| --- | --- |
| **Claude Code** | The terminal chat tool that can read files, create files, run commands, and use subagents. |
| **CLAUDE.md** | Project-wide instructions Claude Code loads automatically in every session. |
| **Skill** | Reusable instructions for doing one task well. |
| **Agent / subagent** | A named specialist role with a responsibility and its own context window. |
| **Tool** | An action Claude can take, such as reading a file, editing a file, running `curl`, or calling an API. |
| **Orchestration** | One lead session coordinating smaller specialist subagents and combining their results. |
| **Evidence** | Public data, links, API results, or saved files that support a claim. |
| **Evidence trail** | A readable folder showing what the system fetched, summarized, and used. |
| **Grounded report** | A report that separates observations, assumptions, and uncertainty. |
| **Signal** | A public hint that claim activity *might* change. Not a forecast. |

## Skill Or Agent?

| Concept | Use it for | Example |
| --- | --- | --- |
| **Skill** | How to do a repeatable task. | `weather-hazard-lookup`, `report-writer` |
| **Subagent** | Who owns a responsibility. | `hazard-scout`, `evidence-reviewer` |
| **Tool** | What action the system can take. | `curl`, file read/write, API call |

Simple rule:

- **Skill**: how should this task be done?
- **Agent**: who should own this part of the work?
- **Tool**: what action can the system take?

For example, a `risk-analyst` agent might use a `weather-hazard-lookup` skill.
The agent owns the analysis.
The skill explains how to use that data source.
The tool is the actual `curl` command or API call.

## What You Will Build

By the end, your repo should contain:

```text
CLAUDE.md
.claude/
  agents/
    one-or-more-custom-agents.md
  skills/
    multiple-skill-folders/
      SKILL.md
outputs/
  runs/
    2026-10-14-1430-berlin-claims-signals/
      README.md
      evidence/
      claims-signal-report.md
```

Your final report should answer:

> Which upcoming public events or conditions in Europe should an insurance
> claims or risk analyst look at because they may drive unusual claim activity?

Important: public data can suggest risk signals. It does not prove future claims.

## Rough Timing

Use this as a guide, not a rule.

- **0:00-0:30**: Open your workspace, start Claude Code, and play with the warm-up examples.
- **0:30-1:00**: Learn skills and create your first own skill.
- **1:00-1:45**: Learn agents and create your first own agent.
- **1:45-2:30**: Explore public data sources yourself.
- **2:30-3:30**: Build skills and agents for the claims-signal challenge.
- **3:30-4:00**: Generate, review, and improve the final report.
- **4:00+**: Add another agent, HTML output, or a use case from your own work.

## 0. Open Your Workspace

Every participant gets their **own workspace**: a VS Code editor running in your browser,
on a machine in the cloud. Claude Code and this lab are already installed there.
You do not install anything on your laptop.

1. Open the workspace link from the facilitators in your browser and sign in.
2. Wait until you see VS Code with the file list on the left.

✅ On the left you see `README.md`, `CLAUDE.md`, `outputs`, and `templates`.

Your workspace is personal and keeps your files, even if you close the browser tab or take a break.
Just open the same link again.

**A quick tour of the screen:**

| Area | What it is |
| --- | --- |
| **Explorer** (left) | All files of the lab. Click a file to open it. Folders starting with a dot, like `.claude`, are visible here too. |
| **Editor** (middle) | Where files open. For Markdown files, right-click the tab → **Open Preview** to see them nicely formatted. |
| **Terminal** (bottom) | Where you type commands. Open it with **Terminal → New Terminal** in the menu (☰ top left), or `` Ctrl+` ``. |

Tip: open this README in preview on one side, and keep the terminal at the bottom. Then you can read and work at the same time.

**Copy and paste in the terminal:** in a browser, `Ctrl+C` / `Ctrl+V` sometimes does not work inside the terminal.
Use `Ctrl+Shift+C` / `Ctrl+Shift+V` (Mac: `Cmd+C` / `Cmd+V`), or right-click → **Paste**.
The first time, your browser may ask whether the page may access the clipboard: choose **Allow**.

## 1. Start Claude Code

Open a terminal (**Terminal → New Terminal**). It already starts in the lab folder.
A terminal is a text window: you type a command, press Enter, and it answers.
In this lab you only need a handful of commands, and you can copy each of them from this guide.

✅ Type `ls` and press Enter. You see `README.md`, `CLAUDE.md`, and `templates`.

If you do not, type `cd ~/workshop-agent-lab` (or ask a facilitator for the folder name).

Start Claude Code:

```bash
claude
```

On the first start, Claude Code may ask a few questions:

1. **Theme.** Pick any.
2. **Login.** If Claude Code asks you to log in, follow the instructions from the facilitators.
   (Usually your workspace is already logged in, and this question does not appear.)
3. **Do you trust this folder?** Choose **yes**. Only then does it load `CLAUDE.md`, the settings,
   skills, and agents from this repo.

✅ You see a prompt box at the bottom of the terminal. Type `Hi, what is in this folder?` and press Enter.
Claude answers and mentions the README and the templates.

Then try these commands, one at a time:

```text
/skills
/agents
/permissions
/status
```

These show what Claude Code loaded from the repository, and which account and model you use.
Press `Esc` to close a menu and return to the chat.

To quit Claude Code, type `/exit`. To start it again, type `claude`.
If you want to continue your last conversation, use `claude --continue`.

### When Claude Asks For Permission

Several times per step, Claude stops and shows a box like *"Do you want to ...?"* with options.

- **Yes**: allow this one action.
- **Yes, and don't ask again ...**: fine for reading files or harmless commands such as fetching public weather data.
- **No** (or `Esc`): refuse, and tell Claude what to do instead.

Read the box before you answer. This box is the "human in the loop" the whole lab is about.

Note: Claude Code runs in the terminal, not in a chat panel of the editor. If VS Code shows other AI
chat or Copilot features, ignore them for this lab.

## 2. Open A Second Terminal

Some steps (for example the Ticketmaster key, or trying a `curl` command yourself)
are easier **outside** Claude Code.
Keep Claude Code running and open a second terminal: click the **+** icon in the terminal panel
(or **Terminal → New Terminal** again). Switch between them in the list on the right side of the panel.

From now on:

- commands in grey `bash` boxes go into the **second terminal**
- everything you say to Claude goes into the **first one**, where Claude Code runs

## 3. Warm-Up: Skill And Agent

This repo starts with one unrelated example:

```text
.claude/skills/poem-writer/SKILL.md
.claude/agents/poet.md
```

The example is intentionally not about insurance. It lets you learn the mechanics first,
without worrying about the domain.

In Claude Code, try:

> Use the poem-writer skill to write a 6-line poem about a rainy Monday commute.

Then try the agent:

> Use the poet agent to write a short poem about an analyst spotting a pattern in the data.

You can use agents and skills interchangeably in prompts:

- tell Claude to use a specific skill, or invoke it directly with `/poem-writer`
- tell Claude to use a specific agent, or mention it with `@poet`
- ask normally and let Claude infer what fits from the descriptions

The files are plain Markdown. Open them and inspect how little structure is needed.

## 4. Create Your First Skill

Write the first skill yourself. This is the moment where you learn what a skill actually is.

Skills are best for repeatable instructions that should only appear when relevant.
They are not global behavior rules. Global rules belong in `CLAUDE.md`.

Think of a skill as a reusable method or checklist.
It usually does not own the whole problem.
It helps an agent or the main Claude Code session do one task consistently.

Good skills are:

- **narrow**: one job, not five
- **easy to trigger**: the description says when to use it
- **specific**: clear steps, rules, and output format
- **safe**: no hidden secrets, no unnecessary command execution
- **testable**: you can ask Claude to use it and judge the result

Create a new folder and file:

```text
.claude/skills/concise-summarizer/SKILL.md
```

You can copy the structure from:

```text
templates/skill-template.md
```

Write a simple skill for summarizing long findings into five business-readable bullets.

Minimum content:

```text
---
name: concise-summarizer
description: Use this skill to summarize long findings into five business-readable bullets.
license: MIT
---

# Concise Summarizer

Use this skill when ...

Rules:

- Use exactly five bullets.
- Use plain business language.
- Keep each bullet under 20 words.
```

Skill-writing tips:

- Put the most important behavior in the `description`; Claude uses it to decide when the skill is relevant.
- Use lowercase names with hyphens, for example `concise-summarizer`.
- Write instructions as if you were briefing a smart colleague.
- Add one example prompt so future users know how to invoke it.
- Avoid vague rules like "be good" or "be accurate." Say what accuracy means.

After you wrote it, ask Claude to review it:

> Review my concise-summarizer skill.
> Is it clear when to use it?
> Would you follow the rules correctly?
> Suggest improvements, but do not edit the file yet.

Claude Code picks up skills from `.claude/skills/`. Check that it sees your new one
with `/skills`, or ask:

> Which skills are available in this project?

If the new skill does not show up, exit (`/exit` or `Ctrl+C` twice) and restart Claude Code:

```bash
claude
```

Checkpoint:

> Use my new skill on README.md and show me what it does.

Then improve the skill once:

> The output was close, but I want it to be more useful for business stakeholders.
> Suggest three improvements to the skill instructions before I edit them.

## 5. Create Your First Agent

In Claude Code, custom agents are called **subagents**.
Create one either through the CLI or by writing a file.

Agents are best for specialist roles.
A good agent has a clear job, clear boundaries, and a clear moment when it should be used.

Think of an agent as a teammate with a job title.
It can use skills, tools, and context to complete its part of the work.

Use an agent when you want:

- a named role, such as `hazard-scout` or `evidence-reviewer`
- a separate context window for a focused subtask
- a repeatable way of handling a larger workflow step
- tool limits, for example read-only review versus editing

Avoid creating an agent when a short skill would be enough.

Friendly CLI flow:

```text
/agents
```

In the agents menu, choose **Create new agent**, then **Project** (so it is saved in this repo).
Then choose **Manual configuration** instead of generating it with Claude.
This is important for the lab: you should write the agent instructions yourself,
because that is where the learning happens.

Project agents live here, so your team can share them via git:

```text
.claude/agents/
```

File flow:

```text
.claude/agents/<your-agent-name>.md
```

You can copy:

```text
templates/agent-template.md
```

If you create the file by hand, restart Claude Code (or open `/agents` again) so it is picked up.

Both flows are valid.
The CLI flow is friendlier.
The file flow makes the structure visible and easier to version.

Agent-writing tips:

- Choose a short lowercase name with hyphens.
- Make the `description` concrete. This helps Claude decide when to delegate to the agent.
- Give the agent one main responsibility.
- Tell it what not to do.
- Tell it what output to return.
- Start with fewer tools. Add more only when the agent needs them.
- If you leave out `tools`, the agent inherits all tools from the main session.

### Agent Description Clinic

The `description` is more important than it looks.
Claude uses it to decide when the agent should be used.

Use this pattern:

| Part | Question to answer |
| --- | --- |
| Task | What does this agent do? |
| Situation | When should someone use it? |
| Trigger words | What might a user ask for? |
| Boundary | What should this agent not do? |

Weak:

> Analyzes events and risk.

Stronger:

> Use this agent to identify upcoming public events and weather conditions
> in European cities that may create insurance claim signals.
> Trigger phrases: claims watchlist, risk signal, upcoming hazards, event exposure.
> Do not claim that public data proves that claims will rise.

In your group, compare two descriptions.
Which one would Claude understand more reliably?

### Choose Tools Deliberately

An agent should only get tools it needs for its responsibility.
Start small, then add more when the workflow proves it needs them.

| Tool capability | Claude Code tool names | Useful when the agent needs to... |
| --- | --- | --- |
| Read files | `Read` | Inspect skills, evidence, or prior reports. |
| Edit files | `Write`, `Edit` | Save reports, summaries, or evidence notes. |
| Search locally | `Grep`, `Glob` | Find files or text inside the repo. |
| Run commands | `Bash` | Query APIs with `curl` or process local files. |
| Use web access | `WebSearch`, `WebFetch` | Discover or inspect public online sources. |
| Use skills | `Skill` | Load a skill's instructions inside the agent. |

Note: subagents cannot spawn their own subagents.
Delegation always goes from the main session to a subagent.

Before adding a tool, ask:

- What action does this agent need to take?
- Could a narrower skill be enough?
- What could go wrong if this tool is used carelessly?

For example:

```text
---
name: evidence-reviewer
description: Reviews claims-signal reports for unsupported claims,
missing sources, and overconfident wording.
tools: Read, Grep, Glob
model: sonnet
---

You are an evidence reviewer.

Check for:

- unsupported claims
- missing sources
- unclear date ranges or geography
- overconfident language (e.g. "claims will rise")

Return:

- issues to fix
- safer wording
- final risk level: low, medium, or high
```

Checkpoint:

> Use my new agent to complete a small task. Then explain which instructions it followed.

Then test whether the agent is easy to trigger:

> I have a short report draft. Which agent in this repo should review it, and why?

## 6. Understand Orchestration

In this lab, orchestration means:

```text
Lead Claude Code session
  -> delegates to specialist subagents
  -> specialist agents use skills
  -> tools or shell commands fetch evidence
  -> lead combines everything into a report
```

Useful commands:

```text
/agents   browse, create, or edit subagents
/memory   inspect or edit CLAUDE.md
/context  inspect what is loaded into the context window
/tasks    inspect background tasks
/model    switch model
```

Subagents can run in parallel. Ask for it explicitly, for example:
"Run the hazard-scout and attention-analyst agents in parallel."

Important idea:

The lead Claude Code session does not have to do everything itself.
It can delegate focused work to specialist agents.
Those agents can use skills when their descriptions match the task.
This keeps the lead session focused on planning, judgment, and final synthesis.

For the final challenge, a useful team could be:

- the main session (guided by `CLAUDE.md` or a lead skill) that coordinates the investigation
- one hazard/event-search agent
- one public-attention agent
- one evidence-review agent
- skills for specific data sources or report formats

You decide the actual design.

Good orchestration usually has four parts:

1. **Planner**: decides what needs to be investigated.
2. **Collectors**: gather evidence from public sources.
3. **Reviewer**: challenges weak or unsupported claims.
4. **Writer**: creates a clear report for humans.

For this workshop, start small:

```text
main session (lead)
  -> hazard scout
  -> evidence reviewer
  -> report writer skill
```

Because subagents cannot delegate further, the **main session is the lead**.
Put the lead's playbook in `CLAUDE.md` or in a `claims-signal-lead` skill.
Alternatively, start Claude Code with `claude --agent claims-signal-lead`
to make your lead agent the main session for that run.

Only add more agents when the work is truly different.

Orchestration prompts should be explicit:

```text
Act as the lead for this task.
Delegate hazard and event discovery to the hazard-scout agent.
Use the evidence-reviewer agent before writing the final report.
Save all outputs in a timestamped folder under outputs/runs/.
```

### Evidence Trail

Every agent that retrieves data should leave a readable evidence trail.

Create one timestamped folder per run:

```text
outputs/runs/YYYY-MM-DD-HHMM-short-description/
```

Example:

```text
outputs/runs/2026-10-14-1430-berlin-claims-signals/
  README.md
  evidence/
    open-meteo-berlin-summary.md
    open-meteo-berlin-raw.json
    holidays-germany-summary.md
  claims-signal-report.md
```

The timestamped folder should be understandable for non-technical people.

Include a `README.md` in each run folder with:

- what question was investigated
- which agents and skills were used
- which data sources were queried
- where to find the final report
- known limitations

Data-source agents should save:

- a raw response when useful, usually `.json`
- a short human-readable summary, usually `.md`
- the query, source, date range, and time retrieved
- empty or failed results too, so debugging is possible

Common mistakes:

- creating many agents with overlapping jobs
- writing vague descriptions, so Claude cannot choose the right agent
- skipping the reviewer step
- letting the report make stronger claims than the evidence supports
- hiding data retrievals inside the final report only
- adding API keys or secrets to files

Useful references:

- Skills: [Claude Code docs: Agent Skills](https://code.claude.com/docs/en/skills)
- Subagents: [Claude Code docs: Subagents](https://code.claude.com/docs/en/sub-agents)
- Memory / CLAUDE.md: [Claude Code docs: Memory](https://code.claude.com/docs/en/memory)
- CLI flags: [Claude Code docs: CLI reference](https://code.claude.com/docs/en/cli-reference)

## 7. Explore Data Sources Yourself

Before looking at suggestions, spend time searching for data sources.

Your question:

> What public data could indicate that an upcoming event or condition
> might drive unusual insurance claim activity in Europe?

Look for sources with:

- future dates
- location or city
- a severity or size signal (storm intensity, event size, holiday travel peak)
- usable access from a browser or API
- clear limitations

Ask Claude to help, but make it show sources and tradeoffs:

> Help me find public data sources for upcoming European hazards and events
> that may affect insurance claim activity.
> Prioritize sources I can query without authentication.
> Give me source, signal type, access method, and limitation.

Claude Code can search the web itself (`WebSearch` and `WebFetch` tools).
For a deeper source sweep, ask it to research in plan mode (`Shift+Tab`) so it explores without editing files:

```text
Research public data sources for upcoming European hazards and events that could affect insurance claim activity.
Focus on sources with dates, locations, severity signals, and usable access.
Return source links, access method, strengths, and limitations.
```

Use web research to **find and compare sources**, not to skip the lab.
Afterwards, turn the best source ideas into your own skills.

For now, do not use hardcoded demo data.
Use only live public sources and public APIs.

Do not build yet. First compare options with your group.

## 8. Sources We Found Useful

After your own exploration, compare against these. All are public and most need no authentication.

**Open-Meteo**
Weather forecast and historical weather context: storms, heat, cold, wind, precipitation.
Strong signal for property, motor, and health claims. No auth.

**Nager.Date**
Public holidays by country and year. Useful for travel peaks and higher road traffic,
which can shift motor and travel claims. No auth.

**GDELT**
News/event signal for unusual public attention, disruptions, protests, strikes, floods,
or wildfires. No auth but noisy and rate-limited.

**Wikimedia Pageviews**
Attention signal for places, events, or topics (e.g. a city after a storm).
A soft proxy for public interest. No auth.

**Ticketmaster Discovery API**
Upcoming concerts, sport, and large gatherings by city and date.
Why it matters for insurance: a large gathering concentrates people and traffic in
one place at one time, which can lift exposure across several lines at once —
personal accident and injury, public liability for the venue or organiser,
opportunistic theft (contents and travel), motor incidents around the venue, and
event-cancellation cover itself. A single concert rarely moves an insurer's numbers,
but a cluster of big events in a city over a short window is a signal worth flagging.
API-key access may be needed (see below).

You do not need all five. Pick a small set and make it work.

### Optional: Ticketmaster API Key

Ticketmaster is optional for this challenge. It provides information about future
public gatherings: event names, dates, cities, venues, categories, and links —
useful when you want to reason about the crowd-related exposure described above
(accident, injury, liability, theft, and event-cancellation).

It also earns its place for a second reason: it is the only source here that needs
an API key. That makes it the natural point in the lab to practise **handling a
secret safely** — a core question for any real agentic system. Store the key as an
environment variable, pass it to the tool at run time, and never write it into a
skill, an agent file, a report, or anything you commit. If your agent needs the key,
it should read it from the environment, not from a file in the repo.

If your group wants it, one more technical colleague can create the developer app
and share the key within the group for the workshop.

If Ticketmaster setup is blocked, continue with the no-auth sources
(weather, holidays, news) and state the limitation in your report.

To get one:

1. Go to the Ticketmaster Developer Portal: `https://developer.ticketmaster.com/`
2. Create or sign in to a developer account.
3. Create an app/project.
4. Copy the Consumer Key / API key for the Discovery API.
5. Store it as an environment variable in your workspace terminal.

```bash
export TICKETMASTER_API_KEY="<YOUR_TICKETMASTER_KEY>"
```

An `export` only applies to the terminal you typed it in.
Claude Code only sees the key if you start `claude` **from that same terminal** afterwards,
so quit Claude Code (`/exit`), run the `export`, then start `claude` again.
Never paste the key into the Claude chat.

Test it in the terminal:

```bash
CITY="Berlin"
START="2026-10-01"
END="2026-12-31"

curl -sS "https://app.ticketmaster.com/discovery/v2/events.json" \
  --get \
  --data-urlencode "apikey=${TICKETMASTER_API_KEY}" \
  --data-urlencode "city=${CITY}" \
  --data-urlencode "startDateTime=${START}T00:00:00Z" \
  --data-urlencode "endDateTime=${END}T23:59:59Z" \
  --data-urlencode "size=5" \
  --data-urlencode "sort=date,asc"
```

A no-auth starting point you can run right away is the weather forecast for a city.
For example, Berlin:

```bash
curl -sS "https://api.open-meteo.com/v1/forecast" \
  --get \
  --data-urlencode "latitude=52.52" \
  --data-urlencode "longitude=13.405" \
  --data-urlencode "daily=temperature_2m_max,temperature_2m_min,precipitation_sum,wind_speed_10m_max" \
  --data-urlencode "forecast_days=14" \
  --data-urlencode "timezone=Europe/Berlin"
```

Non-developer shortcut:

> I want to check the 14-day weather forecast for Berlin using Open-Meteo.
> Explain each command before running it, then summarize any days
> that look like they could drive weather-related claims.

## 9. Choose A Capability Direction

You do not all need to build the same thing.
Choose a capability that your group finds interesting.

Here are possible directions.

### Capability A: Forward Claims-Signal Watchlist

Build a system that finds the top three upcoming public events or conditions
in Europe to watch over the next three months.

The system should answer:

- what are the signals (storm, heatwave, holiday travel peak, large event)?
- where and when are they happening?
- what public evidence supports their relevance?
- which lines of business might they touch (property, motor, travel, health, liability)?
- what confidence level should a human analyst assign?

This is the recommended default direction.

### Capability B: Historical Backtest

Build a system that looks at a known past event and asks:

> What public signals were visible before the claims arrived?

Examples could include a past windstorm, a flood, a heatwave, or a major public holiday.

The system should answer:

- what signals were visible 14, 7, or 3 days before?
- which sources would have helped?
- what would the agent have flagged?
- what internal data would be needed to confirm whether claims actually changed?

This direction is useful for learning how to validate an agent instead of only trusting its future recommendations.

> **Bring your own problem.** If your team finishes early, sketch an agentic
> system for a challenge from your own daily work — a customer-service assistant
> that reads across policy, billing, and claims notes, or an assistant that drafts
> the next step in a dunning (Mahnverfahren) case. Keep it as a design plus one or
> two skills; you do not need real data to demonstrate the idea.

## 10. Build An Agentic System

Now build an agentic system that can autonomously create a grounded claims-signal report.

There is no perfect number of skills or agents.
Part of the exercise is to experiment with the design and discover what actually helps.

Recommended starting point:

- **three to five skills**
- **two to four agents**
- one timestamped run folder under `outputs/runs/`
- one final report inside that run folder
- readable evidence files inside that run folder

For example, you might build:

```text
skills:
  weather-hazard-lookup
  public-attention-signal
  report-writer

agents:
  claims-signal-lead
  evidence-reviewer
```

Or, if your group wants more specialization:

```text
skills:
  open-meteo-forecast
  holiday-calendar
  gdelt-news-signal
  grounded-report-writer
  evidence-checker

agents:
  claims-signal-lead
  hazard-scout
  attention-analyst
  evidence-reviewer
```

At least:

- create at least **two skills**
- create at least **one custom agent**
- generate a timestamped run folder under `outputs/runs/`
- save data retrievals and summaries inside that run folder
- generate a report inside that run folder
- include evidence, confidence, and limitations

Experiment with questions like:

- Is this better as a skill or an agent?
- Does this agent have a clear separate responsibility?
- Are two agents doing the same job?
- Would one stronger skill be simpler than another agent?
- Does the lead session (CLAUDE.md or lead skill) know when to delegate?

Example prompt:

> Use our agents and skills to find upcoming public events or conditions in Europe
> in the next 90 days that may deserve attention from an insurance claims or risk analyst.
> Focus on signals that could plausibly shift claim activity.
> Create a timestamped run folder under `outputs/runs/`.
> Save readable evidence files and a grounded markdown report there.
> Include sources, affected lines of business, confidence, and limitations.

If you are not sure what to build, start with this:

> Help our group design a simple agentic system.
> We want one lead (main session) plus one subagent and two skills.
> Ask us three short questions, then create the first draft files.

If your team is faster, extend the system:

- create an HTML report
- add a source-specific skill
- add an evidence-review agent
- build a second agentic system for a problem from your own work

### Run, Diagnose, Improve

Your first run will probably not be perfect.
That is the point.
Use the first output to improve your skills and agents.

After each run, ask:

| What to inspect | What good looks like |
| --- | --- |
| Evidence trail | Raw results and readable summaries were saved. |
| Source quality | Claims link back to public sources or saved evidence. |
| Report format | The report follows the structure your team requested. |
| Confidence | The system explains uncertainty instead of hiding it. |
| Scope | The agent did not invent claim volumes or loss amounts. |

Common fixes:

| Problem | Possible fix |
| --- | --- |
| Signals are too small or irrelevant. | Add clearer inclusion criteria to the hazard/event-search skill. |
| Sources are missing. | Tell the agent to save links, queries, and retrieval times. |
| Report is too speculative. | Strengthen the evidence-reviewer agent. |
| Output format drifts. | Add exact headings or table columns to the report skill. |
| Agents overlap too much. | Merge roles or make responsibilities sharper. |

Useful prompt:

> Review our latest run folder.
> Find the weakest part of the agentic system.
> Suggest three changes to our skills or agents before we run it again.

## 11. Theory To Practice Map

Use this table to connect the morning concepts to what you built.

| Concept | Where it appears in this repo |
| --- | --- |
| Prompting | The wording inside `CLAUDE.md`, `SKILL.md`, and agent files. |
| Tools | The actions Claude can take: `Read`, `Edit`, `Grep`, `Bash`, `WebFetch`, and more. |
| Skills | Reusable task instructions under `.claude/skills/`. |
| Agents | Specialist subagents under `.claude/agents/`. |
| Orchestration | The main session coordinating skills and subagents. |
| RAG | Retrieved public evidence added to the final reasoning. |
| Evaluation | The evidence-review step and the run-diagnose-improve loop. |

## 12. Report Checklist

Before you are done, your report should include:

- signal name (hazard, event, or condition)
- location
- date or date range
- source
- which lines of business it might affect
- why it might matter for claim activity
- confidence level
- limitations
- recommended human follow-up

Use cautious language:

> This is a public risk signal, not a claims forecast.

Avoid:

> This proves claims will increase.

## 13. Useful Commands

Run a one-off prompt non-interactively (print mode):

```bash
claude -p "Summarize README.md in three bullets."
```

Run with a specific agent as the main session:

```bash
claude --agent poet \
  -p "Write a 4-line poem about clean data."
```

Pre-approve specific tools for a non-interactive run
(safer than skipping all permission checks):

```bash
claude -p "Fetch the Berlin forecast from Open-Meteo and save a summary in outputs/." \
  --allowedTools "Bash(curl:*)" "Write" "Read"
```

Start interactively, or continue your last conversation:

```bash
claude
claude --continue
```

Inspect what is loaded:

```text
/skills
/agents
/permissions
/status
/context
```

## 14. When You Get Stuck

Try one of these prompts:

> Inspect this repository and explain the next smallest step.

> Help me improve my agent description so Claude knows when to use it.

> Review my skill. Is it specific enough for a model to follow?

> My report feels too speculative. Help me make the wording more evidence-based.

Good agent systems are usually not born perfect. You improve them by making responsibilities clearer.
