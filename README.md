# Workshop Agent Lab — Emerging Risk & Claims Signals

Welcome. In this lab you will build an agentic system with GitHub Copilot CLI.

The final goal is a grounded report about upcoming public events and conditions
in Europe that could create **unusual insurance claim activity**.
The system should find signals, evaluate them, and explain what a human analyst
in claims, risk, or underwriting should look at next.

You will not start by writing an agent framework. You will build with Copilot CLI-native building blocks:

- **skills**: reusable instructions for a capability
- **agents**: specialist roles with their own instructions and tool access
- **the main Copilot session**: the coordinator that plans, delegates, and combines results
- **public data sources**: evidence for the report

The README is the main guide. Move through it at your own pace.

For this version of the lab, use live public sources, public APIs,
or allowed internal data.
Do not rely on prebuilt demo data unless a facilitator explicitly adds it later.

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

## What Is GitHub Copilot CLI?

GitHub Copilot CLI is a chat-based AI assistant that runs in your terminal.
Instead of only suggesting code inside an editor, it can work with the files in this folder and help execute a workflow.

In this lab, Copilot CLI can:

| Capability | What it means here |
| --- | --- |
| Chat | You give it natural-language instructions. |
| Read files | It can inspect this README, skills, agents, and outputs. |
| Write files | It can create skills, agents, summaries, and reports. |
| Run commands | It can call tools such as `curl` when you approve. |
| Use skills | It can load reusable instructions for specific tasks. |
| Use agents | It can delegate work to specialist roles. |

Think of it as the **workspace where your agentic system runs**.
You will design the skills and agents; Copilot CLI will use them to do the work.

Important: Copilot CLI is powerful because it can read files, edit files, and run commands.
Review what it plans to do, especially before allowing terminal commands or using API keys.

## Which Copilot Mode Should You Use?

In this lab, Copilot CLI has two jobs:

1. help you **build** skills and agents
2. help you **run** the agentic system you built

That can feel confusing at first, because both happen in the same terminal.
Use the mode based on what you are trying to do.

| Mode | Use it for |
| --- | --- |
| **Interactive** | Default. Run your agentic system step by step and stay in control. |
| **Plan** | Ask Copilot to think through a design before changing files. |
| **Autopilot** | Let Copilot continue working more independently on a clear task. |

Recommendation for this workshop:

Use **interactive mode** as the default, especially while your group is still learning.
It is the easiest way to inspect evidence, discuss results, and correct the system.

Use **autopilot** later if your agentic system is already clear and you want Copilot
to run the full workflow more independently.

Useful commands:

```text
/plan       plan before changing files
/autopilot  toggle more autonomous work
```

## Which Model Should You Use?

Use a fast, low-cost model by default.
This lab is mostly about designing skills, agents, evidence trails, and reports.
You do not need the strongest model for every step.

| Task | Recommended model style |
| --- | --- |
| Writing skills and agents | Fast, cheap model |
| Fetching and summarizing evidence | Fast, cheap model |
| Reviewing weak evidence | Stronger model if available |
| Final report polish | Stronger model if available |

Good default:

- use a Haiku-class model or a GPT mini-class model if your setup offers one
- use a stronger model only when the output quality clearly needs it

If you use the LiteLLM fallback, a facilitator may provide a model name.
Set it with:

macOS / Linux:

```bash
export COPILOT_MODEL="<MODEL_NAME>"
```

Windows PowerShell:

```powershell
$env:COPILOT_MODEL = "<MODEL_NAME>"
```

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

You will use a terminal, but most steps can be done by asking Copilot CLI to create or edit files for you.
If terminal commands are unfamiliar, work in pairs and copy the commands exactly.

Use this rule of thumb:

- **Green path**: use Copilot prompts, inspect what it created, and discuss the result.
- **Yellow path**: edit Markdown files by hand if you are comfortable.
- **Red path**: write code only if your team wants to go further.

In this lab, Markdown files are just text files.
Skills and agents are mostly structured text, not software engineering.

## Tiny Glossary

| Term | Short meaning |
| --- | --- |
| **Copilot CLI** | The terminal chat tool that can read files, create files, run commands, and use agents. |
| **Skill** | Reusable instructions for doing one task well. |
| **Agent** | A named specialist role with a responsibility. |
| **Tool** | An action Copilot can take, such as reading a file, editing a file, running `curl`, or calling an API. |
| **Orchestration** | One lead agent coordinating smaller specialist agents and combining their results. |
| **Evidence** | Public data, links, API results, or saved files that support a claim. |
| **Evidence trail** | A readable folder showing what the system fetched, summarized, and used. |
| **Grounded report** | A report that separates observations, assumptions, and uncertainty. |
| **Signal** | A public hint that claim activity *might* change. Not a forecast. |

## Skill Or Agent?

| Concept | Use it for | Example |
| --- | --- | --- |
| **Skill** | How to do a repeatable task. | `weather-hazard-lookup`, `report-writer` |
| **Agent** | Who owns a responsibility. | `hazard-scout`, `evidence-reviewer` |
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
.github/
  agents/
    one-or-more-custom-agents.agent.md
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

- **0:00-0:30**: Start Copilot CLI and play with the warm-up examples.
- **0:30-1:00**: Learn skills and create your first own skill.
- **1:00-1:45**: Learn agents and create your first own agent.
- **1:45-2:30**: Explore public data sources yourself.
- **2:30-3:30**: Build skills and agents for the claims-signal challenge.
- **3:30-4:00**: Generate, review, and improve the final report.
- **4:00+**: Add another agent, HTML output, or a use case from your own work.

## 1. Start Copilot CLI

Open a terminal in this folder:

macOS / Linux:

```bash
cd workshop-agent-lab
```

Windows PowerShell:

```powershell
cd workshop-agent-lab
```

Check that Copilot CLI is installed:

```bash
copilot --version
```

If you see `command not found` or a similar error, stop here and ask a facilitator.
Do not spend workshop time debugging installation alone.

If this works, start Copilot:

```bash
copilot
```

Then try:

```text
/env
/skills
/agent
```

These commands show what Copilot loaded from the repository.
If one of these commands opens a view or menu, press `Esc` to return to the chat.

## 2. Authenticate

Use GitHub SSO as the default path:

```bash
copilot login
```

Follow the browser flow and sign in with your GitHub account.

If SSO does not work, ask a facilitator for the LiteLLM fallback details. Then use:

macOS / Linux:

```bash
export COPILOT_PROVIDER_BASE_URL="<LITELLM_BASE_URL>/v1"
export COPILOT_PROVIDER_API_KEY="<WORKSHOP_KEY>"
export COPILOT_MODEL="<MODEL_NAME>"
```

Windows PowerShell:

```powershell
$env:COPILOT_PROVIDER_BASE_URL = "<LITELLM_BASE_URL>/v1"
$env:COPILOT_PROVIDER_API_KEY = "<WORKSHOP_KEY>"
$env:COPILOT_MODEL = "<MODEL_NAME>"
```

Then test:

```bash
copilot -p "Reply with exactly READY" \
  --allow-all-tools \
  --silent
```

If you are on a managed laptop and something fails, work with a partner or ask a facilitator.
The lab is designed so teams can share one working setup.

## 3. Warm-Up: Skill And Agent

This repo starts with one unrelated example:

```text
.github/skills/poem-writer/SKILL.md
.github/agents/poet.agent.md
```

The example is intentionally not about insurance. It lets you learn the mechanics first,
without worrying about the domain.

In Copilot CLI, try:

> Use the poem-writer skill to write a 6-line poem about a rainy Monday commute.

Then try the agent:

> Use the poet agent to write a short poem about an analyst spotting a pattern in the data.

You can use agents and skills interchangeably in prompts:

- tell Copilot to use a specific skill
- tell Copilot to use a specific agent
- ask normally and let Copilot infer what fits

The files are plain Markdown. Open them and inspect how little structure is needed.

## 4. Create Your First Skill

Write the first skill yourself. This is the moment where you learn what a skill actually is.

Skills are best for repeatable instructions that should only appear when relevant.
They are not global behavior rules. Global rules belong in `.github/copilot-instructions.md`.

Think of a skill as a reusable method or checklist.
It usually does not own the whole problem.
It helps an agent or the main Copilot session do one task consistently.

Good skills are:

- **narrow**: one job, not five
- **easy to trigger**: the description says when to use it
- **specific**: clear steps, rules, and output format
- **safe**: no hidden secrets, no unnecessary command execution
- **testable**: you can ask Copilot to use it and judge the result

Create a new folder and file:

```text
.github/skills/concise-summarizer/SKILL.md
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

- Put the most important behavior in the `description`; Copilot uses it to decide when the skill is relevant.
- Use lowercase names with hyphens, for example `concise-summarizer`.
- Write instructions as if you were briefing a smart colleague.
- Add one example prompt so future users know how to invoke it.
- Avoid vague rules like "be good" or "be accurate." Say what accuracy means.

After you wrote it, ask Copilot to review it:

> Review my concise-summarizer skill.
> Is it clear when to use it?
> Would you follow the rules correctly?
> Suggest improvements, but do not edit the file yet.

Reload skills after creating or changing them:

```text
/skills reload
```

If that does not show the new skill, restart Copilot:

```bash
copilot
```

Check:

```text
/skills
```

Checkpoint:

> Use my new skill on README.md and show me what it does.

Then improve the skill once:

> The output was close, but I want it to be more useful for business stakeholders.
> Suggest three improvements to the skill instructions before I edit them.

## 5. Create Your First Agent

Create an agent either through the CLI or by writing a file.

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
/agent
```

In the agent menu, press `n` to create a new agent.
Then choose **Create manually**.
This is important for the lab: you should write the agent instructions yourself,
because that is where the learning happens.

Create it in the project so your team can share it:

```text
.github/agents/
```

File flow:

```text
.github/agents/<your-agent-name>.agent.md
```

You can copy:

```text
templates/agent-template.agent.md
```

Both flows are valid.
The CLI flow is friendlier.
The file flow makes the structure visible and easier to version.

Agent-writing tips:

- Choose a short lowercase name with hyphens.
- Make the `description` concrete. This helps Copilot decide when to use the agent.
- Give the agent one main responsibility.
- Tell it what not to do.
- Tell it what output to return.
- Start with fewer tools. Add more only when the agent needs them.

### Agent Description Clinic

The `description` is more important than it looks.
Copilot uses it to decide when the agent should be suggested or used.

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
Which one would Copilot understand more reliably?

### Choose Tools Deliberately

An agent should only get tools it needs for its responsibility.
Start small, then add more when the workflow proves it needs them.

| Tool capability | Useful when the agent needs to... |
| --- | --- |
| Read files | Inspect skills, evidence, or prior reports. |
| Edit files | Save reports, summaries, or evidence notes. |
| Search locally | Find files or text inside the repo. |
| Run commands | Query APIs with `curl` or process local files. |
| Use web access | Discover or inspect public online sources. |
| Use other agents | Delegate a focused subtask to a specialist. |

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
tools: ["read", "search"]
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
Lead Copilot session
  -> delegates to specialist agents
  -> specialist agents use skills
  -> tools or shell commands fetch evidence
  -> lead combines everything into a report
```

Useful commands:

```text
/agent    browse or create agents
/skills   inspect available skills
/tasks    inspect subagents and commands
/fleet    enable parallel subagent execution
/env      inspect loaded instructions, skills, agents, and tools
```

Important idea:

The lead Copilot session does not have to do everything itself.
It can delegate focused work to specialist agents.
Those agents can use skills when their descriptions match the task.
This keeps the lead session focused on planning, judgment, and final synthesis.

For the final challenge, a useful team could be:

- one lead agent that coordinates the investigation
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
lead agent
  -> hazard scout
  -> evidence reviewer
  -> report writer skill
```

Only add more agents when the work is truly different.

Orchestration prompts should be explicit:

```text
Use the lead agent to coordinate this task.
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
- writing vague descriptions, so Copilot cannot choose the right agent
- skipping the reviewer step
- letting the report make stronger claims than the evidence supports
- hiding data retrievals inside the final report only
- adding API keys or secrets to files

Useful references:

- Skills: [GitHub docs: adding agent skills](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/add-skills)
- Agents: [GitHub docs: creating custom agents](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli)
- Comparison: [GitHub docs: comparing CLI features](https://docs.github.com/en/copilot/concepts/agents/copilot-cli/comparing-cli-features)

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

Ask Copilot to help, but make it show sources and tradeoffs:

> Help me find public data sources for upcoming European hazards and events
> that may affect insurance claim activity.
> Prioritize sources I can query without authentication.
> Give me source, signal type, access method, and limitation.

You can also use Copilot CLI research mode for source discovery:

```text
/research Find public data sources for upcoming European hazards and events that could affect insurance claim activity.
Focus on sources with dates, locations, severity signals, and usable access.
Return source links, access method, strengths, and limitations.
```

Use research mode to **find and compare sources**, not to skip the lab.
Afterwards, turn the best source ideas into your own skills.

For now, do not use hardcoded demo data.
The default path is live public sources, public APIs, or allowed internal data.

Do not build yet. First compare options with your group.

## Optional: Enrich With Internal Data

If your group has internal data you are allowed to use, you can enrich the agentic system with it.

Examples:

- Databricks tables or SQL queries
- Excel or CSV files
- internal APIs
- dashboard exports
- historical claims counts, policy exposure by region, or catastrophe reserves

This is a great place to create a dedicated skill.

For example:

- `databricks-claims-query`
- `excel-exposure-loader`
- `internal-api-evidence`
- `claims-baseline-summary`

Rules for internal data:

- only use data you are allowed to access
- do not commit secrets, tokens, or private data into the repo
- save readable summaries in the timestamped run folder
- describe the source and limitation without exposing sensitive details
- keep raw internal data out of the repo unless you are sure it is allowed

Useful prompt:

> We have an internal export with claim counts by region and month.
> Help us design a skill that reads or summarizes it safely.
> The skill should save a readable evidence summary,
> avoid exposing sensitive rows in the final report,
> and state what internal validation is still needed.

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
Useful for crowd-related liability, accident, or event-cancellation exposure.
Strong signal, but API-key access may be needed.

You do not need all five. Pick a small set and make it work.

### Optional: Ticketmaster API Key

Ticketmaster is optional for this challenge. It provides information about future
public gatherings: event names, dates, cities, venues, categories, and links —
useful when you want to reason about crowd-related exposure.

If your group wants it, one more technical colleague can create the developer app
and share the key within the group for the workshop. Do not commit the key into any file.

If Ticketmaster setup is blocked, continue with the no-auth sources
(weather, holidays, news) and state the limitation in your report.

To get one:

1. Go to the Ticketmaster Developer Portal: `https://developer.ticketmaster.com/`
2. Create or sign in to a developer account.
3. Create an app/project.
4. Copy the Consumer Key / API key for the Discovery API.
5. Store it as an environment variable.

macOS / Linux:

```bash
export TICKETMASTER_API_KEY="<YOUR_TICKETMASTER_KEY>"
```

Windows PowerShell:

```powershell
$env:TICKETMASTER_API_KEY = "<YOUR_TICKETMASTER_KEY>"
```

Test with a browser or terminal:

macOS / Linux:

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

Windows PowerShell:

```powershell
$CITY = "Berlin"
$START = "2026-10-01"
$END = "2026-12-31"

$params = @{
  apikey = $env:TICKETMASTER_API_KEY
  city = $CITY
  startDateTime = "${START}T00:00:00Z"
  endDateTime = "${END}T23:59:59Z"
  size = 5
  sort = "date,asc"
}

Invoke-RestMethod `
  -Method Get `
  -Uri "https://app.ticketmaster.com/discovery/v2/events.json" `
  -Body $params
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

### Capability C: Public Signals Plus Internal Context

Build a system that combines public signals with allowed internal or mock internal data.

Examples:

- policy exposure or number of insured customers by region
- historical claim baselines by month or peril
- reserve or capacity context
- Excel, CSV, Databricks, or internal API summaries

The system should answer:

- how does internal context change the interpretation?
- which public signals still look relevant?
- which signals become less important?
- what should a human analyst check next?

Use this direction only with data you are allowed to access and summarize.
Do not commit secrets or sensitive data.

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
- Does the lead agent know when to delegate?

Example prompt:

> Use our agents and skills to find upcoming public events or conditions in Europe
> in the next 90 days that may deserve attention from an insurance claims or risk analyst.
> Focus on signals that could plausibly shift claim activity.
> Create a timestamped run folder under `outputs/runs/`.
> Save readable evidence files and a grounded markdown report there.
> Include sources, affected lines of business, confidence, and limitations.

If you are not sure what to build, start with this:

> Help our group design a simple agentic system.
> We want one lead agent and two skills.
> Ask us three short questions, then create the first draft files.

If your team is faster, extend the system:

- create an HTML report
- add a source-specific skill
- add an internal-data skill for Databricks, Excel, or an internal API
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
| Prompting | The wording inside `SKILL.md` and `.agent.md` files. |
| Tools | The actions Copilot can take: read, edit, search, run commands, use web access. |
| Skills | Reusable task instructions under `.github/skills/`. |
| Agents | Specialist roles under `.github/agents/`. |
| Orchestration | The lead session or lead agent coordinating skills and agents. |
| RAG | Retrieved public or internal evidence added to the final reasoning. |
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

Run a specific agent in prompt mode:

macOS / Linux:

```bash
copilot --agent poet \
  -p "Write a 4-line poem about clean data." \
  --allow-all-tools \
  --silent
```

Windows PowerShell:

```powershell
copilot --agent poet `
  -p "Write a 4-line poem about clean data." `
  --allow-all-tools `
  --silent
```

Run from this repo explicitly:

macOS / Linux:

```bash
copilot -C . \
  --agent poet \
  -p "Write a 4-line poem about clean data." \
  --allow-all-tools \
  --silent
```

Windows PowerShell:

```powershell
copilot -C . `
  --agent poet `
  -p "Write a 4-line poem about clean data." `
  --allow-all-tools `
  --silent
```

Start interactively:

```bash
copilot
```

Inspect what is loaded:

```text
/env
/skills
/agent
```

## 14. When You Get Stuck

Try one of these prompts:

> Inspect this repository and explain the next smallest step.

> Help me improve my agent description so Copilot knows when to use it.

> Review my skill. Is it specific enough for a model to follow?

> My report feels too speculative. Help me make the wording more evidence-based.

Good agent systems are usually not born perfect. You improve them by making responsibilities clearer.
