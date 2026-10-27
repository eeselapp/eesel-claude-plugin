---
name: review-support-performance
description: Review what an eesel AI support agent has been doing and what customers ask about, from eesel's data. Use when the user asks what their eesel agent did today or this week, wants support ticket trends, top themes or recurring issues from their eesel-connected helpdesk, wants to know which questions the agent couldn't answer or handed off, wants a support report from eesel, or asks what to fix next to make their eesel agent better.
---

# Review the agent's work and the support queue

## Connecting eesel

The eesel connector's `status` tool answers once eesel is connected. If its tools aren't available: in claude.ai or Cowork, connect **eesel** from this plugin's **Connectors** tab (on Team and Enterprise plans, a Claude organization Owner adds it first); in Claude Code, run `/mcp`, choose `plugin:eesel:eesel` and pick **Authenticate**. Sign-in happens on eesel's own page, which also creates a free trial workspace (100 credits for 7 days, no card) for someone without one, so passwords, API keys and tokens never go through the chat.

The connector works as one agent: the one picked when eesel was connected, or the workspace's first agent if none was picked. Every tool, `chat` included, acts as that agent, except the `activity` list, which shows runs from every agent in the workspace, each with its agent's name. For another agent, reconnect eesel and pick it on the sign-in page, or use the eesel dashboard. Tool names here are the connector's own, and your client may show them with a prefix.

## How to approach it

- **What the agent did is in its activity runs,** one per job it worked. They give the detail and examples of that work; pass on each run's dashboard link (its `url`, on dashboard.eesel.ai) so the person can check any of it. Counting them by hand covers only the span you read, so say which span that is. Keep each agent's runs apart. Totals for a period come from asking the agent in `chat`.
- **Ticket trends and gaps come from asking the agent,** which works across their tickets. A big analysis is heavy work and can use credits, and the agent may email the report to the workspace owner, so the person decides on the size and where it goes before you send the request. If the agent says it can't run an analysis in their workspace, tell the person, rather than building trends from activity runs, which only cover the tickets the agent worked.
- **Which fixes to apply is the person's call,** because each fix changes what the agent does for customers. Missing knowledge can also come from connecting the source it lives in (the `set-up-support-agent` skill).
- **When the agent gets something wrong, check with the person before correcting it in `chat`,** because it saves editors' lasting corrections, and the saved correction then shapes its replies to customers.

## Reference

- The list shows runs newest first, and a read of one shows what started it, what the agent answered, and which actions it took or held.
- Example requests, through `chat`: "Analyze our tickets from the last 30 days. Surface the top themes with their share of volume, how each changed compared with the 30 days before, and two real examples per theme." Or: "Which questions did you fail to answer or hand off this week? For each one, say what knowledge or instruction would fix it."
- The agent can estimate a job's cost if you ask it in `chat`, and `billing` shows the plan and credits used. Most tickets and customer chats are 1 credit each; heavy ones use more, a scheduled run uses at least 1, each image the agent generates is 1, and a very long chat with the agent itself uses credits past its free allowance. So credits used isn't a ticket count.
