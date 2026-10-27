---
name: handle-approvals
description: Review, approve or deny actions an eesel AI support agent is holding for a person, such as a customer reply, a refund, a tag or a ticket update. Use when the user asks what is waiting for their approval in eesel, wants to approve or deny a held action, or when an eesel reply says an action is waiting for approval.
---

# Approve or deny held actions

An action set to **Needs approval** is prepared by the agent and held until someone decides. Approving runs it right away in the system it targets: the reply reaches the customer, the refund goes out. It can't be undone from here.

## Connecting eesel

The eesel connector's `status` tool answers once eesel is connected. If its tools aren't available: in claude.ai or Cowork, connect **eesel** from this plugin's **Connectors** tab (on Team and Enterprise plans, a Claude organization Owner adds it first); in Claude Code, run `/mcp`, choose `plugin:eesel:eesel` and pick **Authenticate**. Sign-in happens on eesel's own page, which also creates a free trial workspace (100 credits for 7 days, no card) for someone without one, so passwords, API keys and tokens never go through the chat.

The connector works as one agent: the one picked when eesel was connected, or the workspace's first agent if none was picked. Every tool, `chat` included, acts as that agent, except the `activity` list, which shows runs from every agent in the workspace, each with its agent's name. For another agent, reconnect eesel and pick it on the sign-in page, or use the eesel dashboard. Tool names here are the connector's own, and your client may show them with a prefix.

## How to approach it

- **The person decides each held action, seeing exactly what it will do:** the app, the customer or ticket, and the content, such as the reply text or the refund amount. It acts in their real system the moment it's approved, so the decision is theirs, made on those details, and a batch approval covers only what they've seen listed.
- **Anything that moves money, such as a refund, isn't switched to Auto from here.** Anthropic's directory policy requires a person in the loop for financial actions taken through a plugin. The person can change that setting in the eesel dashboard.
- **Only workspace editors (Owners and Editors) can decide.** Viewers see held actions but can't release them, so a permissions error means someone else has to.
- **Which actions need approval is the team's setting,** changed in the eesel dashboard or by asking the agent in `chat`.

## Reference

- `approvals` lists each held action, what it would do, and its `approval_id`. `decide_approval` with `action: "approve"` or `"deny"` and the `approval_id` decides one; after a denial, the agent carries on without it.
- These cover the connected agent. Another agent's held actions are decided in the eesel dashboard at [dashboard.eesel.ai](https://dashboard.eesel.ai).
- Each action's setting is **Auto**, **Needs approval** or **Disabled**, set in the dashboard under **Integrations**, on the app's actions. **Always allow** on an approval in the dashboard also switches that action to **Auto**.
- A connection that returned `waiting_for_approval` may not be listed in `approvals`. `status` shows whether it went through, and the agent in `chat` can give the connect link.
