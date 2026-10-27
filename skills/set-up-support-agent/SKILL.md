---
name: set-up-support-agent
description: Set up an eesel AI customer support agent inside the user's helpdesk. Use when the user wants eesel, or wants a ready-made AI agent or AI teammate for customer support without naming another product. That covers connecting a helpdesk such as Zendesk, Freshdesk, Gorgias, Help Scout, HubSpot, Front or Zoho Desk, training an agent on past tickets, macros and help center articles, or having an AI agent draft and automate replies inside their helpdesk. Not for writing your own support bot code, or for drafting a single reply yourself.
---

# Set up an eesel AI support agent in a helpdesk

eesel is an AI teammate for customer support. It learns from a team's help center, macros, past tickets and files, then drafts or sends replies in their helpdesk, tags and routes tickets, and hands off what needs a person. The goal here is a team that has seen the agent get their own tickets right and has it drafting on new ones.

## Connecting eesel

The eesel connector's `status` tool answers once eesel is connected. If its tools aren't available: in claude.ai or Cowork, connect **eesel** from this plugin's **Connectors** tab (on Team and Enterprise plans, a Claude organization Owner adds it first); in Claude Code, run `/mcp`, choose `plugin:eesel:eesel` and pick **Authenticate**. Sign-in happens on eesel's own page, which also creates a free trial workspace (100 credits for 7 days, no card) for someone without one, so passwords, API keys and tokens never go through the chat.

The connector works as one agent: the one picked when eesel was connected, or the workspace's first agent if none was picked. Every tool, `chat` included, acts as that agent, except the `activity` list, which shows runs from every agent in the workspace, each with its agent's name. For another agent, reconnect eesel and pick it on the sign-in page, or use the eesel dashboard. Tool names here are the connector's own, and your client may show them with a prefix.

## How to approach it

- **Start from what the workspace already has.** A helpdesk connected elsewhere in the workspace can't be used by this agent until it's added to it, and adding it needs no new sign-in. So offer to add it to this agent, rather than connecting it from scratch.
- **Ask for one thing to begin: a link to their helpdesk or help center,** and work out the helpdesk and subdomain from it. A link is something they already have, and most people who left eesel's setup did so at the first question, usually a choice between ways to connect or a request for a subdomain. A help center on their own domain usually names its helpdesk in the page, so read it before asking.
- **If they have no helpdesk, don't pick one for them; start from what they want done,** such as answering visitors on their website (the `add-website-chat-bubble` skill). A helpdesk they don't have is a dead end.
- **Prefer the full connection.** Drafts on their tickets, automations and actions all need it. The help-center-only option (`quick_start`), where a helpdesk offers one, suits someone who can't, or doesn't want to, connect the helpdesk account yet.
- **The person approves a connection inside their helpdesk,** through the link that `execute` returns. Keys for helpdesks that connect with an API key go into eesel's secure form, never the chat, because a key pasted here stays in the chat history: ask the agent in `chat` to connect it and pass on the form link it returns, or the person uses **Integrations** in the dashboard.
- **Which rules the agent follows (when to escalate, what it may promise) is the team's call,** so ask them, then pass the rules to the agent in `chat`.
- **Show it working on tickets they know before anything is switched on.** Offer a few recent tickets still waiting on a reply, each named by what the customer asked so the person picks one without hunting for it, or the agent's answers to recently resolved tickets next to what their team sent. Their team's answer beside the agent's is the fastest proof, and asking the agent to show drafts here, not post them, keeps the helpdesk untouched. A bigger test on past tickets is the `test-on-past-tickets` skill.
- **Offer a narrow first automation that drafts rather than replies, unless they ask for direct replies,** on the kind of ticket they watched the agent get right, because they trust it on tickets they've already seen it handle. When they haven't said where drafts should go, a private note or draft on each new ticket is a sensible default: drafts that stay in the dashboard barely automate anything. If they started on drafts, direct replies come after those drafts have been landing well and the person asks. A second drafting automation on the same tickets would give each ticket two drafts, so check what's already on first.
- **Turning an automation on changes what happens on live tickets,** because it runs on every matching ticket from then on, and each ticket it works usually uses 1 credit. So the person decides, knowing that.
- **Which actions need approval is the team's setting, and refunds and other actions that move money are never switched to Auto from here.** Anthropic's directory policy requires a person in the loop for financial actions taken through a plugin. The person can change any setting in the eesel dashboard.
- **When the agent gets something wrong, check with the person before correcting it in `chat`,** because it saves editors' lasting corrections, and the saved correction then shapes its replies to customers.

## Reference

- `integrations` (default `action: "list"`): integrations on this agent. `other_workspace_integrations` are connected in the workspace but not on this agent. `manage_integrations` with `action: "execute"`, the `platform` and that `integration_id` adds one to this agent.
- `integrations` with `action: "apps"` and `platform` (such as `zendesk`): connection options, each with a `type` (`oauth`, `api_key` or `quick_start`), and what each enables.
- Zendesk's detail has a `source_choice`. Pass the sources they decline, comma-separated, as `excluded_sources` in `params`; one excluded now can be brought back later by asking the agent in `chat`.
- `manage_integrations` with `action: "connect"`, `platform` and `connection_type` starts a connection. `action: "execute"` with `params` (such as `{"zendeskSubdomain": "acme"}`) completes it.
- A connect that returns `waiting_for_approval` with no link: ask the agent in `chat` for "the connect link for <platform>". `status` confirms a connection once it's approved.
- `integrations` with `action: "download_list"` or `"download_status"` and an `integration_id`: what can be downloaded and how far it has got. Connecting usually starts the first download, which takes minutes for most ticket histories and longer for a big one, and the agent can already answer from what has landed.
- `manage_integrations` with `action: "download"`, `integration_id` and `type` (such as `tickets` or `articles`) starts a download. With a `url`, it crawls a website instead. Asking the agent in `chat` to read the site also works.
- `automations` with `integration_key` (such as `zendesk`) lists that helpdesk's automations, each with a `key`, and which are on.
- `manage_automations` takes that `integration_key`, and the automation's `key` as `trigger_key`.
- A helpdesk automation sees the ticket as it is when the event fires, so a condition that lands later, such as a tag a person adds, can't gate it, unless that tag is itself the event. A schedule that sweeps the queue can.
- Credits: setup is free, and so are most chats with the agent (a very long one uses credits past its free allowance). Most tickets and customer chats are 1 credit each; heavy ones use more, a scheduled run uses at least 1, and each image the agent generates is 1. Plans are on [eesel's pricing page](https://www.eesel.ai/pricing).
- Dashboard: [dashboard.eesel.ai](https://dashboard.eesel.ai). Each action's setting (**Auto**, **Needs approval** or **Disabled**) is under **Integrations**, on the app.
