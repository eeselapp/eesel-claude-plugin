# eesel AI: Support Agent for Helpdesks and Websites

![eesel AI](assets/icon.png)

Build an AI support agent that answers tickets in your helpdesk and chats with visitors on your website, without leaving Claude. eesel learns from your past tickets, help center and internal docs. It drafts or sends replies, tags and routes tickets, and hands off anything that needs a person. It works with the helpdesk you already use, including Zendesk, Freshdesk and Gorgias, connects to 3,000+ other apps, and replies in more than 80 languages.

## Skills

| Skill | What it does |
| --- | --- |
| `set-up-support-agent` | Connects your helpdesk, loads your help center, macros and past tickets, sets the agent's rules, and turns on drafting, starting with private notes or drafts your team reviews. |
| `add-website-chat-bubble` | Adds eesel's AI chat bubble, an AI chatbot, to your website. In a codebase it finds the stack and adds the script to the right layout file. For Shopify, WordPress, Webflow and other hosted platforms, it gives the exact steps. |
| `test-on-past-tickets` | Runs your agent on a sample of real resolved tickets without acting on them, compares its answers with what your team sent, and reports match rates by theme with the fixes that would raise them. |
| `review-support-performance` | Shows what your agent did, the top themes and trends in your tickets, and the questions it couldn't answer, with what to fix next. |
| `handle-approvals` | Lists the actions your agent is holding for a person, such as a reply or a refund, and approves or denies each one only when you say so. |

## Try it

- "Set up an AI support agent for our Zendesk at acme.zendesk.com."
- "Add an AI support chatbot to this site."
- "Test my agent on last month's tickets and show me where it falls short."
- "What did my eesel agent do this week?"
- "What's waiting for my approval?"

## Connect your account

- The plugin includes the eesel AI connector at `https://oracle.eesel.app/mcp`. It's the same server as the eesel AI connector in Claude's connector directory, so you get one set of tools, not two.
- In claude.ai or Cowork, connect it from the plugin's **Connectors** tab. In Claude Code, run `/mcp`, choose `plugin:eesel:eesel`, and pick **Authenticate**.
- You sign in on eesel's own page through OAuth, and no credentials are shared with Claude. If you don't have an account, the same page creates a free trial workspace: 100 credits for 7 days, no card needed.
- Everything follows your eesel workspace permissions. Viewers can read, editors can change things, and write actions follow the approval settings you've configured in eesel.

## What it runs and where data goes

- **What's in the plugin:** Markdown skills, one reference file, an icon, and one remote MCP server entry. There is no local server, hook, script, package install or credential in it.
- **Where tool calls go:** only to the eesel AI connector at `https://oracle.eesel.app/mcp`, which acts as your eesel agent. Inputs can include your messages to the agent, website addresses, text you choose to upload, app and subdomain names, and approval decisions. Results can include ticket and conversation content, customer names and email addresses, drafts, reports and account usage, depending on your permissions.
- **What the agent does in your apps:** it acts in the apps you connect to eesel, such as your helpdesk, Slack or Shopify, when you ask it to or when an automation you turned on runs. Anything set to need approval waits for a person. Some jobs, such as a test on past tickets, end with the agent emailing a report to the workspace owner or posting it in a connected Slack channel, and the skills tell you before they start one.
- **Files on your machine:** in Claude Code, the `add-website-chat-bubble` skill reads your project's config and layout files to find where the chat script goes. It adds a script tag that loads `https://cdn.eesel.ai/loader.js` to that layout file, and for Next.js it may create `.env.local` to hold the chat's public id. It may run your site locally to check the chat appears. It doesn't send your project files to eesel.
- **Public web pages:** to work out which helpdesk you use, or what your site is built with, Claude may read your public help center or website page, if it can fetch web pages.
- **What's stored:** knowledge, instructions, files and conversation history you create are stored in your eesel workspace, as described in eesel's privacy policy. The plugin doesn't read unrelated chats or memory.
- **Who it's for:** businesses and their support teams. It isn't intended for people under 18.

## Pricing

The agent uses 1 credit per ticket or chat conversation it handles for customers, however many messages it takes. Setup is free. Chatting with your agent is free too, except that a very long chat uses credits past its free allowance. There are no per-seat fees. Plans: https://www.eesel.ai/pricing

## Links

- [Documentation](https://docs.eesel.ai/apis-and-developer-resources/use-eesel-from-claude-and-other-assistants)
- [Privacy policy](https://www.eesel.ai/privacy)
- [Terms](https://www.eesel.ai/terms)
- [Support](https://www.eesel.ai/contact): hi@eesel.app
- [eesel AI website](https://www.eesel.ai)

## License

MIT. See [LICENSE](LICENSE).
