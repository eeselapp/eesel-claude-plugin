---
name: set-up-support-agent
description: Set up an eesel AI customer support agent inside the user's helpdesk. Use when the user wants eesel, or wants a ready-made AI agent or AI teammate for customer support without naming another product. That covers connecting a helpdesk such as Zendesk, Freshdesk, Gorgias, Help Scout, HubSpot, Front or Zoho Desk, training an agent on past tickets, macros and help center articles, or having an AI agent draft and automate replies inside their helpdesk. Not for writing your own support bot code, or for drafting a single reply yourself.
---

# Set up an eesel AI support agent in a helpdesk

eesel is an AI teammate for customer support. It learns from a team's help center, macros, past tickets and files. Then it drafts or sends replies in the helpdesk, tags and routes tickets, and hands off what needs a person. This skill takes a team from nothing to an agent drafting replies on their real tickets.

## Before you start: connect eesel

Call the eesel connector's `status` tool. If it answers, eesel is connected.

If the eesel tools aren't available:

- **claude.ai or Cowork:** open this plugin's **Connectors** tab and connect **eesel**.
- **Claude Code:** run `/mcp`, choose `plugin:eesel:eesel`, and pick **Authenticate**.

On Team and Enterprise plans, an Owner adds the connector for the organization first, then each member connects with their own account.

Sign-in happens on eesel's own page. For someone with no account, it creates a free trial workspace: 100 credits for 7 days, no card. Never ask for a password, API key or token in chat.

Tool names below are the eesel connector's own (`chat`, `status`, `integrations` and so on). Your client may show them with a prefix. The connector works with the workspace's first agent. In a workspace with several agents, name the one you mean in each `chat` message (ask the agent in `chat` which agents exist). The other tools always act on the first agent, so for anything else on another agent, use `chat` or the eesel dashboard.

## Step 1: see what's already set up

- `status` lists the connected tools and whether their content is downloaded.
- `integrations` (default `action: "list"`) shows the integrations on this agent. Its `other_workspace_integrations` are connected elsewhere in the workspace but not on this agent.
- If the helpdesk is already on this agent, skip to Step 4.
- If it's connected in the workspace but not on this agent, offer to add it by asking the agent in `chat`, instead of connecting it again.

## Step 2: find the helpdesk

Ask for one thing: a link to their helpdesk or help center. Don't ask them to choose a connection method or type a subdomain.

- Work out the helpdesk and subdomain from the link. `acme.zendesk.com` is Zendesk with subdomain `acme`. `acme.freshdesk.com` is Freshdesk. `acme.gorgias.com` is Gorgias.
- A help center on their own domain, such as `help.acme.com`, usually names its helpdesk in the page footer or source. If you can fetch web pages, read that public page before asking.
- If they have no helpdesk, don't pick one for them. Offer the agent on their website instead (the `add-website-chat` skill), or ask what they want done.

## Step 3: connect it

1. Call `integrations` with `action: "apps"` and the `platform`, for example `zendesk`. It lists the connection options, each with a `type` such as `oauth`, `api_key` or `quick_start`, and what each one enables.
2. Prefer the full connection (`oauth` or `api_key`). Drafts on tickets, automations and actions all need it. Some helpdesks also offer `quick_start`, which reads the public help center only. Use it when they can't or don't want to connect the helpdesk account yet.
3. If the detail includes a `source_choice` (Zendesk does), ask which of its sources to import before connecting, such as help center articles, macros and past tickets. Everything imports by default. Pass the keys of the sources they declined, comma-separated, as `excluded_sources` in `params`.
4. Call `manage_integrations` with `action: "connect"`, the `platform`, and `connection_type` set to the option's `type`. If it asks for fields you already know, such as the subdomain, finish with `action: "execute"` and `params`.
5. Give the person the link it returns. They approve the connection inside their helpdesk. Don't wait in a loop for the click.
6. If it returns `waiting_for_approval` with no link, ask the agent for one: `chat` with "give me the connect link for <platform>".
7. For an `api_key` connection, such as Freshdesk, never ask for the key in chat. Point the person to the secure form the connect step links to, or to **Integrations** in the eesel dashboard at [dashboard.eesel.ai](https://dashboard.eesel.ai).
8. Confirm with `status`. It lists every connected tool with its connection health.

## Step 4: give it knowledge

- Connecting usually starts the first download. Check `status` before starting another one.
- `integrations` with `action: "download_list"` and the `integration_id` shows what can be downloaded (help center, macros, tickets). `action: "download_status"` shows how far a download has got.
- To start a download, call `manage_integrations` with `action: "download"`, the `integration_id` and a `type` such as `tickets` or `articles`. A first ticket download usually takes minutes. The agent can already answer from what has landed.
- For their website, ask the agent in `chat`, for example "read our website https://acme.com".
- For a policy or guide that lives nowhere else, call `upload_file` with a file name and the full text. A file with the same name is replaced, so check `files` first.

## Step 5: tell it how to behave

The agent writes its own instructions, so change them by asking it in `chat`. For example:

> Add to your instructions: escalate (internal note, assign to a person, don't reply) when the customer is upset, asks for a manager, or wants a refund over $100.

Ask the person which rules they want. Don't make up policies for them. Show the result with `instructions`.

## Step 6: show it working on real tickets

Prove the agent on tickets the team already knows before turning anything on:

- Ask the agent in `chat` for two or three recent tickets still waiting on a reply, each named by what the customer asked. Offer a draft for one, and ask the agent to show the draft in this conversation, not post it in the helpdesk.
- Or ask it to answer a few recently resolved tickets here and put its answers next to what the team sent.
- Pass the agent's drafts and comparisons on as they are.
- When a small round goes well, offer a bigger test with the `test-on-past-tickets` skill.

## Step 7: turn on drafting

An automation is a helpdesk event plus instructions for one job. Nothing happens on a ticket until one is on. Once it is on, it runs on every matching ticket.

1. Call `automations` with `integration_key`, for example `zendesk`. It lists that helpdesk's automations, each with its `key`, and which are on. If a drafting automation is already on, don't create a second one.
2. Start with drafts, not replies. The safe default is a private internal note on each new ticket for a person to review.
3. Get the person's OK. Tell them it will run on every matching ticket from then on, and each ticket it works uses 1 credit. Then create it through `chat`, for example:

   > Set up a Zendesk automation: when a customer messages, draft a reply as an internal note for a person to review. Skip anything tagged spam.

4. To pause or resume one, call `manage_automations` with `action: "disable"` or `"enable"`, the `integration_key`, and the automation's `key` as `trigger_key`. Use `action: "delete"` only when they ask to delete it.
5. Start narrow, on the kind of ticket they watched the agent get right. Move routine cases to direct replies only after the drafts have been landing well and the person asks for it.

## Permissions and approvals

Each action (reply, tag, refund) either runs on its own, asks a person first, or is off. New agents are safest with replies and anything irreversible set to ask first. Don't change these settings unless the person asks. Never switch refunds or anything else that moves money to run on its own: leave that to the person, in the dashboard under **Integrations**. For actions waiting on a person, use the `handle-approvals` skill.

## What it costs

Setup is free. Chatting with the agent is free too, except that a very long chat uses credits past its free allowance. The agent uses 1 credit per ticket or chat conversation it handles for customers, however many messages it takes. Plans are on [eesel's pricing page](https://www.eesel.ai/pricing).

## Rules

- Ask before anything that reaches customers or changes the helpdesk: turning on an automation, switching drafts to direct replies, approving an action.
- Never ask for or handle passwords, API keys or tokens.
- Every number you report, such as tickets downloaded, comes from a tool result in this conversation.
- Pass the agent's replies on faithfully. If it gets something wrong, tell the person. Correct the agent in `chat` only after they agree, because it may save the correction to its live instructions.
