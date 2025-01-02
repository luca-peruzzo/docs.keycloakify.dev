# Previewing Your Pages in Different Languages

{% hint style="info" %}
This section assume you have read [Testing your Theme Outside of Keycloak](../../testing-your-theme/outside-of-keycloak.md).
{% endhint %}

To preview your component in different languages, create separate stories for each language.

<details>

<summary>Note for pepoles that have opted out of using Storybook</summary>

If you're not using storybook here is how to preview your page in dev mode.

<pre class="language-tsx" data-title="src/main.tsx"><code class="lang-tsx">/* eslint-disable react-refresh/only-export-components */
import { createRoot } from "react-dom/client";
import { StrictMode } from "react";
import { KcPage } from "./kc.gen";

// The following block can be uncommented to test a specific page with `yarn dev`
// Don't forget to comment back or your bundle size will increase
import { getKcContextMock } from "./login/KcPageStory";

if (import.meta.env.DEV) {
    window.kcContext = getKcContextMock({
        pageId: "register.ftl",
        overrides: {
            locale: {
<strong>                currentLanguageTag: "es"
</strong>            }
        }
    });
}

createRoot(document.getElementById("root")!).render(
    &#x3C;StrictMode>
        {!window.kcContext ? (
            &#x3C;h1>No Keycloak Context&#x3C;/h1>
        ) : (
            &#x3C;KcPage kcContext={window.kcContext} />
        )}
    &#x3C;/StrictMode>
);
</code></pre>

</details>

Example:

<pre class="language-tsx" data-title="src/login/pages/Login.stories.tsx"><code class="lang-tsx">import type { Meta, StoryObj } from "@storybook/react";
import { createKcPageStory } from "../KcPageStory";

const { KcPageStory } = createKcPageStory({ pageId: "login.ftl" });

const meta = {
    title: "login/login.ftl",
    component: KcPageStory
} satisfies Meta&#x3C;typeof KcPageStory>;

export default meta;

type Story = StoryObj&#x3C;typeof meta>;

export const Default: Story = {
    render: () => &#x3C;KcPageStory />
};

<strong>export const French: Story = {
</strong><strong>    render: ()=> (
</strong><strong>        &#x3C;KcPageStory
</strong><strong>            kcContext={{
</strong><strong>                locale: {
</strong><strong>                    currentLanguageTag: "fr"
</strong><strong>                }
</strong><strong>            }}
</strong><strong>        />
</strong><strong>    )
</strong><strong>};
</strong>
<strong>export const Spanish: Story = {
</strong><strong>    render: ()=> (
</strong><strong>        &#x3C;KcPageStory
</strong><strong>            kcContext={{
</strong><strong>                locale: {
</strong><strong>                    currentLanguageTag: "es"
</strong><strong>                }
</strong><strong>            }}
</strong><strong>        />
</strong><strong>    )
</strong><strong>};
</strong>
// Other stories ...
</code></pre>

<figure><img src="https://github.com/keycloakify/docs.keycloakify.dev/blob/v11_next/features/.gitbook/assets/image%20(7).png" alt=""><figcaption><p>Viewing the French story of the login page</p></figcaption></figure>

If you want all your story to by by default in an other language you can edit:

{% code title="src/login/KcPageStory.tsx" %}
```tsx
export const { getKcContextMock } = createGetKcContextMock({
  kcContextExtension,
  kcContextExtensionPerPage,
  overrides: {
    locale: {
      currentLanguageTag: "de",
    },
  },
  overridesPerPage: {},
});
```
{% endcode %}

Ok now let's see how to modify the base translation to best fit your usecase or create new translation messages:

{% content-ref url="adding-new-translation-messages-or-changing-the-default-ones.md" %}
[adding-new-translation-messages-or-changing-the-default-ones.md](adding-new-translation-messages-or-changing-the-default-ones.md)
{% endcontent-ref %}
