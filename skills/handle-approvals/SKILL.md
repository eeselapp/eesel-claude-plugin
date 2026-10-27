---
name: handle-approvals
description: Review, approve or deny actions an eesel AI support agent is holding for a person, such as a customer reply, a refund, a tag or a ticket update. Use when the user asks what is waiting for their approval in eesel, wants to approve or deny a held action, or when an eesel reply says an action is waiting for approval.
---

# Approve or deny held actions

An action set to "Needs approval" is prepared by the agent and held until a person decides. Approving runs it right away in the system it targets: a reply reaches the customer, a refund is issued. It can't be undone from here.

## Before you start: connect eesel

Call the eesel connector's `status` tool. If it answers, eesel is connected.

If the eesel tools aren't available:

- **claude.ai or Cowork:** open this plugin's **Connectors** tab and connect **eesel**.
- **Claude Code:** run `/mcp`, choose `plugin:eesel:eesel`, and pick **Authenticate**.

On Team and Enterprise plans, an Owner adds the connector for the organization first, then each member connects with their own account.

Sign-in happens on eesel's own page. For someone with no account, it creates a free trial workspace: 100 credits for 7 days, no card. Never ask for a password, API key or token in chat.

Tool names below are the eesel connector's own (`chat`, `status`, `integrations` and so on). Your client may show them with a prefix. The connector works with the workspace's first agent. In a workspace with several agents, name the one you mean in each `chat` message (ask the agent in `chat` which agents exist). The other tools always act on the first agent, so for anything else on another agent, use `chat` or the eesel dashboard.

## Steps

1. Call `approvals`. It lists each held action, what it would do, and its `approval_id`.
2. Show the person every held action: what it does, in which app, for which customer or ticket, and its exact content, such as the reply text or the refund amount. Don't summarize the details away.
3. Ask for a decision on each one. Approve only on a clear yes for that specific action. "Approve all" is fine only after you have listed every action it covers and the person confirms. Anything that moves money, such as a refund, always needs its own separate yes.
4. Call `decide_approval` with `action: "approve"` or `action: "deny"` and the `approval_id`. Report what ran and what was dropped.
5. Call `approvals` again to show what's left.

## Good to know

- Only editors (an Owner or Editor of the workspace) can decide. Viewers can see held actions but can't decide them. If deciding fails because of permissions, say so.
- A connection that returned `waiting_for_approval` may not appear here. Check `status`, and ask the agent in `chat` for the connect link.
- To make a kind of action stop asking, or start asking, the person changes it in the eesel dashboard (**Integrations**, then the app's actions), or asks the agent in `chat`, for example "ask me before any refund". Don't change these settings unless they ask. Never switch refunds or anything else that moves money to run on its own: leave that to the person, in the dashboard.
