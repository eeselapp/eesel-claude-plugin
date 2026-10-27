---
name: test-on-past-tickets
description: Test an eesel AI support agent on the team's real past tickets before it talks to customers. Use when the user wants to test, evaluate, QA or simulate their eesel agent, asks how accurate it would be, wants its answers compared with what their team actually sent, or asks whether it is ready to go live.
---

# Test the agent on past tickets

eesel can run a simulation. The agent works a sample of tickets the team already resolved as if they were in the queue, deciding what it would reply and do without acting on any of it, then compares its answers with what the team really sent and reports where it falls short.

## Connecting eesel

The eesel connector's `status` tool answers once eesel is connected. If its tools aren't available: in claude.ai or Cowork, connect **eesel** from this plugin's **Connectors** tab (on Team and Enterprise plans, a Claude organization Owner adds it first); in Claude Code, run `/mcp`, choose `plugin:eesel:eesel` and pick **Authenticate**. Sign-in happens on eesel's own page, which also creates a free trial workspace (100 credits for 7 days, no card) for someone without one, so passwords, API keys and tokens never go through the chat.

The connector works as one agent: the one picked when eesel was connected, or the workspace's first agent if none was picked. Every tool, `chat` included, acts as that agent, except the `activity` list, which shows runs from every agent in the workspace, each with its agent's name. For another agent, reconnect eesel and pick it on the sign-in page, or use the eesel dashboard. Tool names here are the connector's own, and your client may show them with a prefix.

## How to approach it

- **It needs the full helpdesk connection,** because it works the team's own resolved tickets, starting from the downloaded ones and fetching the rest from the helpdesk. A help-center-only connection has none to test on (the `set-up-support-agent` skill covers connecting).
- **The sample is theirs to choose.** If they named tickets, a theme or a date range, that's the sample. Otherwise it's a sample of recent resolved tickets sized to their history, and the report says how many it used and from when.
- **A simulation is heavy work and can use credits,** and the agent usually emails the finished report to the workspace owner and may post it in a connected Slack channel. The person decides on the size and where it goes before you send the request.
- **The request asks the agent to take no action and post nothing in the helpdesk,** because these are the team's real tickets, and a reply posted on one reaches the customer.
- **Pass its numbers on as it gave them,** since the point is finding where the agent falls short before their customers do.
- **Each fix changes what the agent does for customers,** so the person picks which to apply. An instruction goes to the agent in `chat`, a missing source gets connected, and a missing policy can go in with `upload_file`, or the team can write it in their help center. Re-testing the weak themes afterwards shows whether a fix worked.
- **Going live is their call.** When they ask, suggest a narrow drafting automation on the themes that matched well, since they trust the agent on the kind of ticket they've watched it get right, and leave the weak themes out of it until their fixes are in.

## Reference

- `billing` shows the credits left. The agent can estimate a run's cost if you ask it in `chat`.
- An example request, through `chat`: "Run a simulation on our recent resolved tickets. Decide what you would reply and do on each, but take no action and post nothing in our helpdesk. Compare each answer with what our team sent. Group the results by theme, with each theme's share of the sample and its match rate, and one or two side-by-side examples with links. Then list the specific fixes that would raise the match rate." Add the sample, and anything about not emailing or posting the report, as the person decides.
- A report the agent saved as a file shows in `files`.
