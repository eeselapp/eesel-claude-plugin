---
name: add-website-chat
description: Add eesel's AI chat bubble to a website or web app. It's an AI customer support chatbot that answers visitors from the site's own content and hands off to a person. Use when the user asks to add eesel to their site, or asks to add an AI chatbot, AI support chat or chat widget to a site, store, docs site or app and hasn't named a different chat product. Covers any stack (HTML, React, Next.js, Vue, Nuxt, Astro, SvelteKit, Docusaurus, Rails, Laravel, Django) and platform (Shopify, WordPress, Webflow, Squarespace, Wix, BigCommerce, Magento). Not for building a chat interface or bot from scratch.
---

# Add eesel's AI chat to a website

The eesel chat bubble puts the team's AI agent on their website. It answers visitors from the site's own pages, help center and connected tools, links the page it used, and hands off to a person when it should. On a store with Shopify connected, it can also recommend products and look up orders. The install is one script tag.

If the user named a different chat product, don't use this skill.

## Before you start: connect eesel

Call the eesel connector's `status` tool. If it answers, eesel is connected.

If the eesel tools aren't available:

- **claude.ai or Cowork:** open this plugin's **Connectors** tab and connect **eesel**.
- **Claude Code:** run `/mcp`, choose `plugin:eesel:eesel`, and pick **Authenticate**.

On Team and Enterprise plans, an Owner adds the connector for the organization first, then each member connects with their own account.

Sign-in happens on eesel's own page. For someone with no account, it creates a free trial workspace: 100 credits for 7 days, no card. Never ask for a password, API key or token in chat.

Tool names below are the eesel connector's own (`chat`, `status`, `integrations` and so on). Your client may show them with a prefix. The connector works with the workspace's first agent. In a workspace with several agents, name the one you mean in each `chat` message (ask the agent in `chat` which agents exist). The other tools always act on the first agent, so for anything else on another agent, use `chat` or the eesel dashboard.

## Step 1: get the bubble and its trigger id

Every install needs the bubble's trigger id. Never invent one.

1. **In a codebase, look first.** Search the code for `cdn.eesel.ai/loader.js` or `EESEL_TRIGGER_ID`. If the snippet is already there, don't add a second one.
2. **If the user gave you the trigger id,** use it. You can still ask the agent in `chat` for that bubble's preview link, so the person can try it before it goes live.
3. **Otherwise, ask the agent.** Send through `chat`, for example: "Do we already have a chat bubble for https://example.com? If so, give me its embed snippet and preview link. If not, read the site into your knowledge, set up a chat bubble for it, and give me the embed snippet with its trigger id and a preview link." If the reply has no snippet, ask again in the same conversation by passing its `task_id`.
4. **Or the dashboard.** The snippet is also in the eesel dashboard at https://dashboard.eesel.ai, under **Integrations > Chat Bubble**, in the **Install** section.

If you still have no trigger id, write the code with the placeholder `YOUR_TRIGGER_ID` and tell the person to replace it. For a stack that reads it from an environment variable, such as Next.js, leave the variable unset instead of writing the placeholder into it.

## Step 2: test the answers before visitors see them

The bubble goes live as soon as the snippet is on a published page. On Shopify and other hosted platforms, that's the moment the person saves. So test first:

1. Ask the agent what a visitor would ask, through `chat`: "what does this site sell, and what is the returns policy?"
2. If an answer is missing or wrong, tell the person. With their OK, add the knowledge: `upload_file` for a policy they give you, or ask the agent in `chat` to read more pages.
3. If the site isn't public yet, such as an app that only runs locally, ask for the address it will have, or add its key content with `upload_file`.
4. The preview link from Step 1 lets the person try the bubble before it's on their site.

## Step 3: find what the site is built with

- **A codebase you can see:** check `package.json`, the config files and the folder layout, then follow the matching stack in [references/platforms.md](references/platforms.md).
- **A hosted builder or store** (Shopify, WordPress, Webflow, Squarespace, Wix, BigCommerce, Magento): the person adds the snippet in that platform's admin, not in code. Give them the steps for their platform from [references/platforms.md](references/platforms.md).
- **No code to look at,** such as in claude.ai chat: read the site's public page source to spot the platform, or ask the person what it's built with.

## Step 4: add the snippet

Some stacks need a different shape: Next.js, Docusaurus and Magento among them. For those, use the version in [references/platforms.md](references/platforms.md) instead of the plain snippet below.

This is the plain snippet. Change only `YOUR_TRIGGER_ID`. If the person's own snippet from the dashboard differs, use theirs.

```html
<script>
(function(){
  var script = document.createElement('script');
  script.src = 'https://cdn.eesel.ai/loader.js';
  script.async = true;
  script.onload = function() {
    if (window.EeselWidgetWithTrigger) {
      window.EeselWidgetWithTrigger({ triggerId: 'YOUR_TRIGGER_ID' });
    } else {
      window.__EESEL_WIDGET_V2_CONFIG__ = { EESEL_TRIGGER_ID: 'YOUR_TRIGGER_ID' };
    }
  };
  document.head.appendChild(script);
})();
</script>
```

- Put it once, in the shared layout that every page uses, before `</body>`. Not in a single page component, or the bubble shows on that page only.
- Keep everything else in the file as it is. Add only the snippet, plus any import the stack needs.
- It loads one small file from `cdn.eesel.ai` asynchronously, so it doesn't slow the page.
- If the site sets a strict Content Security Policy, it must allow `https://cdn.eesel.ai` for scripts and connections, and the inline snippet needs the site's nonce or hash like its other inline scripts. Tell the person what needs to change, and ask before editing the policy config yourself.

## Step 5: check it and tell the person

1. If you can run the site, open a page and look for the bubble in the bottom right corner. Click it and ask a question. If you can't run it or open a browser, ask the person to check.
2. Tell the person:
   - which file you changed, and what you added
   - if the trigger id is in an environment variable, that they must add the same variable in their host's settings and redeploy, or the live site shows no bubble. If `.gitignore` doesn't cover `.env.local`, mention it: the trigger id is public, so it isn't a secret, but most teams keep that file out of git.
   - that they can change the look (colour, logo, title, starter questions, position) under **Integrations > Chat Bubble** in the dashboard, or by asking the agent in `chat`. Saved settings reach a live bubble in about 30 seconds.
   - that they can pause the bubble without touching the site's code, by asking the agent in `chat` or switching it off on the dashboard's **Automations** page
   - that each bubble conversation uses 1 credit, however many messages it has
3. Ask when the agent should hand a visitor to a person, and what it should do then, such as create a helpdesk ticket or send an email. Then tell the agent in `chat`. It writes those rules into its instructions and lists them back. If the site already runs Zendesk Messenger or Freshchat, the bubble can hand over to it once Zendesk or Freshdesk is connected to eesel. The person picks it in the bubble's settings in the dashboard.

## Rules

- In a codebase, edit only the layout or template file the snippet belongs in. For a stack that reads the trigger id from an environment variable, such as Next.js, you may also create or update `.env.local`. Ask before touching anything else, adding variables to their hosting, or deploying.
- Never invent a trigger id. Never ask for passwords, API keys or tokens.
