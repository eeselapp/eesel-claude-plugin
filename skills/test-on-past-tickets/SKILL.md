---
name: test-on-past-tickets
description: Test an eesel AI support agent on the team's real past tickets before it talks to customers. Use when the user wants to test, evaluate, QA or simulate their eesel agent, asks how accurate it would be, wants its answers compared with what their team actually sent, or asks whether it is ready to go live.
---

# Test the agent on past tickets

eesel can run a simulation. The agent works a sample of recently resolved tickets as if they were in the queue. It decides what it would reply and do, but takes no action. Then it compares its answers with what the team really sent and reports where it falls short.

## Before you start: connect eesel

Call the eesel connector's `status` tool. If it answers, eesel is connected.

If the eesel tools aren't available:

- **claude.ai or Cowork:** open this plugin's **Connectors** tab and connect **eesel**.
- **Claude Code:** run `/mcp`, choose `plugin:eesel:eesel`, and pick **Authenticate**.

On Team and Enterprise plans, an Owner adds the connector for the organization first, then each member connects with their own account.

Sign-in happens on eesel's own page. For someone with no account, it creates a free trial workspace: 100 credits for 7 days, no card. Never ask for a password, API key or token in chat.

Tool names below are the eesel connector's own (`chat`, `status`, `integrations` and so on). Your client may show them with a prefix. The connector works with the workspace's first agent. In a workspace with several agents, name the one you mean in each `chat` message (ask the agent in `chat` which agents exist). The other tools always act on the first agent, so for anything else on another agent, use `chat` or the eesel dashboard.

## Step 1: check there are tickets to test on

- A helpdesk must be fully connected, with its tickets downloaded. A help-center-only (`quick_start`) connection has no tickets to test on. `status` shows the connections. `integrations` with `action: "download_status"` and the `integration_id` shows how many tickets have landed.
- If there's no helpdesk yet, use the `set-up-support-agent` skill first.
- If tickets are still downloading, tell the person and offer to test on what has landed.

## Step 2: agree on the sample and the cost

- Ask what they want tested, or offer the default: about 20 recent tickets the team resolved, more if they have a lot of history. They can also name a date range, a theme such as refunds or shipping, or specific tickets.
- A simulation is heavy work and can use credits. Ask the agent in `chat` for an estimate, check `billing` for what's left, and get the person's OK before a big run.
- The agent usually emails the finished report to the workspace owner, and may post it in a connected Slack channel. Tell the person before you start. If they don't want it emailed or posted, add that to the request.

## Step 3: ask the agent to run it

Send it through `chat`, for example:

> Run a simulation on our last 20 resolved tickets. Decide what you would reply and do on each, but take no action and post nothing in our helpdesk. Compare each answer with what our team sent. Group the results by theme, with each theme's share of the sample and its match rate, and one or two side-by-side examples with links. Then list the specific fixes that would raise the match rate.

This runs long. When the reply says it's still working and gives a `task_id`, call `chat` later with only that `task_id` to collect the result. Don't send the request again, because that starts the work over.

## Step 4: report back

- Lead with the headline: the one sentence they'd repeat to a colleague.
- For each theme: its share of the sample, its match rate, and an example.
- Then the gaps, each with its fix.
- Keep the agent's numbers exactly as it gave them. Don't round up or soften them. A 60% match rate with three named fixes is more useful than a flattering number.
- If the agent saved the report as a file, `files` lists it. Fetch it with `action: "download"` and its key in `keys`.

## Step 5: act on the fixes

Each fix is usually one of these:

- **An instruction to add:** send it to the agent through `chat`.
- **A source to connect:** use the `set-up-support-agent` skill.
- **A missing policy or article:** add it with `upload_file`, or suggest the team writes it in their help center.

Ask before each change. Then offer to re-test the weak themes.

## Ready to go live?

Suggest starting with a narrow drafting automation on the themes with high match rates (Step 7 of the `set-up-support-agent` skill). Keep themes with low match rates on drafts or hand-off until their fixes are in.
