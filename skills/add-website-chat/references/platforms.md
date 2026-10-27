# Where the eesel snippet goes

Put the snippet once, where it renders on every page, just before `</body>`. Replace `YOUR_TRIGGER_ID` with the bubble's trigger id. Keep everything else in the file as it is.

## Codebases

### Plain HTML or a static site

Paste the snippet into the shared template or include that every page uses. If each page is its own file with no shared template, paste it into every page that should show the bubble.

### Next.js (App Router)

Make two additions to `app/layout.tsx`, and leave the rest of the file as it is (metadata, CSS imports, fonts, providers, `<html>` and `<body>` attributes, prop types).

1. Add the import at the top:

   ```tsx
   import Script from 'next/script'
   ```

2. Add this inside `<body>`, after `{children}`:

   ```tsx
   {process.env.NEXT_PUBLIC_EESEL_TRIGGER_ID && (
     <Script id="eesel-chat-bubble" strategy="lazyOnload">
       {`(function(){var s=document.createElement('script');s.src='https://cdn.eesel.ai/loader.js';s.async=true;s.onload=function(){if(window.EeselWidgetWithTrigger){window.EeselWidgetWithTrigger({triggerId:'${process.env.NEXT_PUBLIC_EESEL_TRIGGER_ID}'})}else{window.__EESEL_WIDGET_V2_CONFIG__={EESEL_TRIGGER_ID:'${process.env.NEXT_PUBLIC_EESEL_TRIGGER_ID}'}}};document.head.appendChild(s)})();`}
     </Script>
   )}
   ```

3. Put the trigger id in `.env.local` as `NEXT_PUBLIC_EESEL_TRIGGER_ID=<trigger id>`. Restart `next dev` after changing it. The value is baked in at build time, so the host needs the same variable, then a redeploy.

With the variable unset, nothing renders, so preview builds can go without the bubble. If the app has more than one root layout (route groups with their own `<html>`), add it to each one. `next/script` keeps the bubble across client-side page changes, and `lazyOnload` loads it once the page has finished loading.

### Next.js (Pages Router)

Add the same import and the same `Script` block, wrapped in the same check, once in `pages/_app.tsx` (or `_app.js`), next to the page component it renders. Use `.env.local` the same way.

### React, Vue or Svelte single-page apps (Vite, Create React App)

Paste the snippet into the root HTML file before `</body>`: `index.html` at the project root for Vite, `public/index.html` for Create React App.

### Nuxt

Add it once in `nuxt.config.ts` under `app.head.script`, with the snippet's code (without the `<script>` tags) as `innerHTML` and `tagPosition: 'bodyClose'`.

### Astro

Paste it into the base layout every page uses, such as `src/layouts/Layout.astro`, before `</body>`. Write the opening tag as `<script is:inline>` so Astro doesn't bundle it.

### SvelteKit

Paste it into `src/app.html` before `</body>`.

### Angular

Paste it into `src/index.html` before `</body>`.

### Docusaurus

`src/theme/Root.js` is a React component, so the HTML snippet can't be pasted into it as is. Load the script in an effect instead. If the project already has a `Root.js`, add only the effect to it.

```jsx
import React, { useEffect } from 'react';

export default function Root({ children }) {
  useEffect(() => {
    if (document.getElementById('eesel-loader')) return;
    var script = document.createElement('script');
    script.id = 'eesel-loader';
    script.src = 'https://cdn.eesel.ai/loader.js';
    script.async = true;
    script.onload = function () {
      if (window.EeselWidgetWithTrigger) {
        window.EeselWidgetWithTrigger({ triggerId: 'YOUR_TRIGGER_ID' });
      } else {
        window.__EESEL_WIDGET_V2_CONFIG__ = { EESEL_TRIGGER_ID: 'YOUR_TRIGGER_ID' };
      }
    };
    document.head.appendChild(script);
  }, []);
  return <>{children}</>;
}
```

### Rails, Laravel, Django and other server-rendered apps

Paste it into the main layout template before `</body>`: `app/views/layouts/application.html.erb` (Rails), the main Blade layout such as `resources/views/layouts/app.blade.php` (Laravel), or the base template that other templates extend, often `templates/base.html` (Django).

## Hosted platforms

The person pastes the snippet in the platform's admin, and the bubble is live from that save. Test the answers first (Step 2 of the skill). Give them these steps.

### Shopify

1. In the store admin, open **Online Store > Themes**.
2. Open the menu next to the live theme and pick **Edit code**.
3. Open **Layout > `theme.liquid`**, paste the snippet just before the closing `</body>` tag, and click **Save**.

Connecting Shopify to eesel as well lets the agent recommend products, check stock and look up orders.

### WordPress, Webflow, Squarespace and Wix

Use the platform's setting for a script that runs on every page, in the footer or at the end of the body. Webflow, Squarespace and Wix only offer custom code on paid plans.

- **WordPress:** the theme's footer scripts setting, or a header and footer scripts plugin.
- **Webflow:** **Site settings > Custom code > Footer code**, then publish.
- **Squarespace:** **Pages > Website Tools > Code Injection**, in the **Footer** box. Older versions have it under **Settings > Advanced > Code Injection**.
- **Wix:** **Settings > Custom Code**, add the code, and place it at the end of the body on all pages.

If a menu has moved, look for "custom code", "footer code" or "code injection" in the platform's settings.

### BigCommerce

Add it in **Storefront > Script manager**, with no theme edits. Steps: https://docs.eesel.ai/integrations/e-commerce/bigcommerce/bigcommerce-embed-chat-bubble

### Magento

The plain snippet never renders on Magento, because Magento's RequireJS sets `define.amd` and the loader takes the wrong path. Use this wrapper, which clears `define.amd` while the loader runs and puts it back after. Where to paste it in the Magento admin: https://docs.eesel.ai/integrations/e-commerce/magento/magento-setup

```html
<script>
(function(){
  var amd = window.define && window.define.amd;
  if (amd) { window.define.amd = null; }
  var script = document.createElement('script');
  script.src = 'https://cdn.eesel.ai/loader.js';
  script.async = true;
  script.onload = function() {
    if (amd) { window.define.amd = amd; }
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

## Anything else

Every platform has a place for a script that runs on every page. The full guide is at https://docs.eesel.ai/integrations/chat-surfaces/chat-bubble
