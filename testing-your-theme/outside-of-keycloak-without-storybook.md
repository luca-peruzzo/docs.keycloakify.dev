---
hidden: true
---

# Outside of Keycloak - Without Storybook

This option for testing your theme is a fallback option if you prefer avoiding introducing Storybook into your project.

To do that, just uncomment the following lines in [your entrypoint](#user-content-fn-1)[^1]:

<pre class="language-typescript" data-title="src/[main|index].tsx?"><code class="lang-typescript">import { createRoot } from "react-dom/client";
import { StrictMode } from "react";
import { KcPage } from "./kc.gen";

// The following block can be uncommented to test a specific page with `yarn dev`
// Don't forget to comment back or your bundle size will increase
<strong>// */
</strong>import { getKcContextMock } from "./login/KcPageStory";

if (import.meta.env.DEV) {
    window.kcContext = getKcContextMock({
        pageId: "register.ftl",
        overrides: {}
    });
}
<strong>// */
</strong>
...
</code></pre>

The `pageId` parameter of the `getKcContextMock` lets you decide what page you want to test.\
The overrides parameter lets you modify the default kcContext mock for the page.\
\
For example you can overwrite the `kcContext.locale.currentLanguageTag` to preview your page in a different language.

<pre class="language-tsx"><code class="lang-tsx">window.kcContext = getKcContextMock({
  pageId: "login.ftl",
<strong>  overrides: {
</strong><strong>    locale: {
</strong><strong>      currentLanguageTag: "zh-CN",
</strong><strong>    },
</strong><strong>  },
</strong>});
</code></pre>

Then start the dev server of your, project:

```bash
npm run dev
# Or 'npm run start' (CRA) or 'npm run serve' (Angular Webpack)
```

<figure><img src="../.gitbook/assets/image (86).png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
When you're done testing, don't forget to comment back the import of the mock. Forgetting to do so will negatively impact the bundle size of your pages.
{% endhint %}

[^1]: Your entrypoint is one of the following depending of your Framwork:

    * src/main.tsx
    * src/index.tsx
    * src/main.ts
    * src/index.ts
    * src/main.ts
