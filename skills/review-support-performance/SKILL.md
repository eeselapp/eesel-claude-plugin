---
name: review-support-performance
description: Review what an eesel AI support agent has been doing and what customers ask about, from eesel's data. Use when the user asks what their eesel agent did today or this week, wants support ticket trends, top themes or recurring issues from their eesel-connected helpdesk, wants to know which questions the agent couldn't answer or handed off, wants a support report from eesel, or asks what to fix next to make their eesel agent better.
---

# Review the agent's work and the support queue

## Before you start: connect eesel

Call the eesel connector's `status` tool. If it answers, eesel is connected.

If the eesel tools aren't available:

- **claude.ai or Cowork:** open this plugin's **Connectors** tab and connect **eesel**.
- **Claude Code:** run `/mcp`, choose `plugin:eesel:eesel`, and pick **Authenticate**.

On Team and Enterprise plans, an Owner adds the connector for the organization first, then each member connects with their own account.

Sign-in happens on eesel's own page. For someone with no account, it creates a free trial workspace: 100 credits for 7 days, no card. Never ask for a password, API key or token in chat.

Tool names below are the eesel connector's own (`chat`, `status`, `integrations` and so on). Your client may show them with a prefix. The connector works with the workspace's first agent. In a workspace with several agents, name the one you mean in each `chat` message (ask the agent in `chat` which agents exist). The other tools always act on the first agent, so for anything else on another agent, use `chat` or the eesel dashboard.

## What the agent did

1. `activity` lists recent runs, newest first, 20 per page. Pass `page` for older ones.
2. `activity` with `action: "read"` and a run's `task_id` shows that run in detail: what started it, what the agent answered, and which actions it took or held.
3. Summarize by kind of work and outcome: answered, drafted, handed off, held for approval, or failed. Include the run ids so the person can look one up.
4. Say how many runs you read and the time span they cover. Don't extend counts beyond what you read.

## What customers ask about

Ask the agent through `chat`, for example:

> Analyze our tickets from the last 30 days. Surface the top themes with their share of volume, how each changed compared with the 30 days before, and two real examples per theme.

- An analysis over many tickets is heavy work and can use credits. Ask the agent in `chat` for an estimate, check `billing` for what's left, and get the person's OK before a big run.
- Long jobs reply that they're still working and give a `task_id`. Call `chat` later with only that `task_id` to collect the result. Don't send the request again, because that starts the work over.
- The agent may email the finished report to the workspace owner. Tell the person before you start. If they don't want it emailed, add that to the request.
- If the agent says it can't run this analysis in their workspace, tell the person rather than working around it.

## What to fix next

Ask the agent through `chat`:

> Which questions did you fail to answer or hand off this week? For each one, say what knowledge or instruction would fix it.

Then go through the fixes with the person. Apply each one only after they agree:

- An instruction: send it to the agent through `chat`. The agent writes its own instructions.
- Missing knowledge: `upload_file` for a policy or guide, or connect the source with the `set-up-support-agent` skill.

## Usage and plan

- `billing` shows the plan and credits used. The agent uses 1 credit per ticket or chat conversation it handles for customers.
- `status` shows the connected tools and whether their content is downloaded and up to date.

## Rules

- Every number comes from a tool result in this conversation. Keep the agent's numbers exactly as it gave them.
- Pass the agent's replies on faithfully. If it gets something wrong, tell the person. Correct the agent in `chat` only after they agree, because it may save the correction to its live instructions.
- Ask before any change to instructions, files or automations.
