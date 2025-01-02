---
icon: css3-alt
---

# CSS Customization

{% hint style="info" %}
This page is a must read, even if you plan on redesinging the pages at the component level you must at least understand how to remove the default CSS styles.
{% endhint %}

## Understanding the CSS class system

Upon inspecting the DOM in storybook you will see that most element have at least a couple of classes applied to them.

* A class starting with `kc`, for example `kcLabelClass`.
* One or more classes with pf-, for example `pf-c-form__label`, `pf-c-form__label-text`

<figure><img src=".gitbook/assets/image (26).png" alt=""><figcaption><p>Inspecting an input label on the login page</p></figcaption></figure>

The classes starting with kc do not have any actual styles applied to them. Their sole purpose is to be a target for you to apply your custom styles.

The classes with pf- are Paterfly classes. [Patternfly](https://v5-archive.patternfly.org/) is a CSS framwork in the like of Bootstrap created by RedHat, it is what the the Keycloak team uses to build all their UIs.

## Applying your custom CSS

{% hint style="danger" %}
Do not edit the any file in the `public/keycloakify-dev-resources` directory. Thoses are resource files used by Storybook for simulating a Keycloak environement during devloppement, thoses files aren't part of your theme.&#x20;
{% endhint %}

To apply your custom CSS style you should use the kc classes to target the components.

{% code title="src/login/main.css" %}
```css
.kcLabelClass {
   border: 3px solid red;
}
```
{% endcode %}

{% tabs %}
{% tab title="React" %}
<pre class="language-tsx" data-title="src/login/KcApp.tsx"><code class="lang-tsx"><strong>import "./main.css";
</strong>// ...
</code></pre>
{% endtab %}

{% tab title="Svelte" %}
<pre class="language-html" data-title="src/login/KcPage.svelte"><code class="lang-html">&#x3C;script lang="ts">
<strong>  import "./main.css";
</strong>  import Template from '@keycloakify/svelte/login/Template.svelte';
  ...
</code></pre>
{% endtab %}
{% endtabs %}

This will be the result:

<figure><img src=".gitbook/assets/image (27).png" alt="" width="375"><figcaption><p>A red border has been applied to every input label</p></figcaption></figure>

<details>

<summary>Having diferent stylesheet for the login page, the register page ext...</summary>

Here we used a global stylesheet that applies to all pages of the login theme however you can also apply stylesheet on a page-by-page basis (a stylesheet for the login page, another stylesheet for the register page ect...).&#x20;

If you are planning to customize the pages using React/Angular/Svelte at the component level it will be very obvious how to do so after reading the [using a component library page](common-use-case-examples/using-a-component-library.md).

However if you plan to stick with with CSS only customization it might not be apparent to you how to do it so here is [a snipet of React code](#user-content-fn-1)[^1] shat show how it can be acheived:

{% code title="src/login/KcPage.tsx" %}
```tsx
import {
    Suspense, 
    lazy,
    useMemo
} from "react";

export default function KcPage(props: { kcContext: KcContext }) {
    const { kcContext } = props;

    const { i18n } = useI18n({ kcContext });

    const classes = useCustomStyles(kcContext);

    return (
        <Suspense>
            {(() => {
                switch (kcContext.pageId) {
                    default:
                        return (
                            <DefaultPage
                                kcContext={kcContext}
                                i18n={i18n}
                                classes={classes}
                                Template={Template}
                                doUseDefaultCss={true}
                                UserProfileFormFields={UserProfileFormFields}
                                doMakeUserConfirmPassword={doMakeUserConfirmPassword}
                            />
                        );
                }
            })()}
        </Suspense>
    );
}

function useCustomStyles(kcContext: KcContext) {
    return useMemo(() => {
        
        // You stylesheet that applies to all pages.
        import("./main.css");
        let classes: { [key in ClassKey]?: string } = {
            // Your classes that applies to all pages
        };

        switch (kcContext.pageId) {
            case "login.ftl":
                // You login page specific stylesheet.
                import("./pages/login.css");
                classes = {
                    ...classes,
                    // Your classes that applies only to the login page
                };
                break;
            case "register.ftl":
                // Your account page specific stylesheet
                import("./pages/register.css");
                classes = {
                    ...classes,
                    // Your classes that applies only to the register page
                };
                break;
            // ...
        }

        return classes;

    }, []);
}
```
{% endcode %}

If this code does not make much sense to you you can check out [this video tutorial](https://www.youtube.com/watch?v=Nkoz1iD-HOA) where this approach is demonstrated in practice.

</details>

<details>

<summary>Using Tailwind</summary>

You'll be of course able to use Tailwind the regular way by applying the utility classes to the React/Angular/Svelte components.\
However be aware that you can also use tailwind without having to modify the pages structures using the `@apply` dirrective. It's demonstrated in [this page](css-customization.md#using-tailwind).

</details>

<details>

<summary>Using Bootstrap or some other CSS framwork</summary>

If you wish to use bootstrap or some other CSS framwork that provide standardized CSS classes you might wonder how to apply thoses classes.\
\
Here is an example with Bootstrap:

```bash
yarn add bootstrap
```

<pre class="language-tsx" data-title="src/login/KcPage.tsx"><code class="lang-tsx"><strong>import "bootstrap/dist/css/bootstrap.min.css";
</strong>import { Suspense, lazy } from "react";
import type { ClassKey } from "keycloakify/login";
import type { KcContext } from "./KcContext";
import { useI18n } from "./i18n";
import DefaultPage from "keycloakify/login/DefaultPage";
import Template from "keycloakify/login/Template";
const UserProfileFormFields = lazy(
    () => import("keycloakify/login/UserProfileFormFields")
);

const doMakeUserConfirmPassword = true;

export default function KcPage(props: { kcContext: KcContext }) {
    const { kcContext } = props;

    const { i18n } = useI18n({ kcContext });

    return (
        &#x3C;Suspense>
            {(() => {
                switch (kcContext.pageId) {
                    default:
                        return (
                            &#x3C;DefaultPage
                                kcContext={kcContext}
                                i18n={i18n}
                                classes={classes}
                                Template={Template}
                                doUseDefaultCss={true}
                                UserProfileFormFields={UserProfileFormFields}
                                doMakeUserConfirmPassword={doMakeUserConfirmPassword}
                            />
                        );
                }
            })()}
        &#x3C;/Suspense>
    );
}

const classes = {
<strong>    kcLabelClass: "form-label col-form-label",
</strong>} satisfies { [key in ClassKey]?: string };
</code></pre>

By writing that you've effectively replaced the Patterfly classes `pf-c-form__label pf-c-form__label-text` by the bootstrap classes `form-label col-form-label`.

dWhat will happen in practice, if you inspect the lement in your browser is that the form label that was previously rendered as:

```html
<label for="username" class="kcLabelClass pf-c-form__label pf-c-form__label-text">
```

Will is now rendered as

```html
<label for="username" class="kcLabelClass form-label col-form-label">
```

</details>

## Removing Some of the Default Styles

Let's consider the "Sign In" button of the login page:

<figure><img src=".gitbook/assets/image (21).png" alt="" width="375"><figcaption><p>The default look of the "Sign In" button </p></figcaption></figure>

Let's see how we can unstyle it so that we can apply our custom styles without having to wory about our CSS conflicting with the default Patternlyfly styles.

<figure><img src=".gitbook/assets/image (22).png" alt="" width="375"><figcaption><p>How the "Sign in" button looks with all the Patterfly styles removed</p></figcaption></figure>

To remove the Patternfly styles we must first inspect the element in our browser:

<figure><img src=".gitbook/assets/image (24).png" alt=""><figcaption><p>Inspecting the CSS classes apllied to the "Sign In" button</p></figcaption></figure>

Doing that we can see what Patterfly classes are applied by default to the standardized element:

* `kcButtonClass`                     get assigned    `pf-c-button`
* `kcButtonPrimaryClass`    get assigned    `pf-m-primary` and `long-pf-btn`
* `kcButtonBlockClass`         get assigned    `pf-m-block`
* `kcButtonLargeClass`         get assigned    `btn-lg`

Since we want to remove all the default styles we can instruct Keycloakify to remove all the classes that gets assigned by default to thoses kc classes:

<pre class="language-tsx" data-title="src/login/KcPage.tsx"><code class="lang-tsx">// ...

const classes = {
<strong>    kcButtonClass: "",
</strong><strong>    kcButtonPrimaryClass: "",
</strong><strong>    kcButtonBlockClass: "",
</strong><strong>    kcButtonLargeClass: ""
</strong>} satisfies { [key in ClassKey]?: string };
</code></pre>

After saving this changes this is what we get:

<figure><img src=".gitbook/assets/image (25).png" alt=""><figcaption><p>All Patternfly classes have been stripped out, resulting in the button being restored to it's default HTML style.</p></figcaption></figure>

Now we can freely apply some custom styles to our button without having Patternfly interfering.

<figure><img src=".gitbook/assets/custom-button.gif" alt="" width="375"><figcaption><p>Button with custom style</p></figcaption></figure>

<details>

<summary>Reveal custom CSS code of this custom button</summary>

{% code title="src/login/main.css" %}
```css
.kcButtonClass {
    padding: 10px 20px;
    font-size: 16px;
    font-weight: bold;
    text-transform: uppercase;
    color: #ffffff;
    background: linear-gradient(45deg, #6a11cb, #2575fc);
    border: none;
    border-radius: 25px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    transition: transform 0.2s, box-shadow 0.2s;
    cursor: pointer;
    width: 100%;
}

.kcButtonClass:hover {
    transform: translateY(-2px);
    box-shadow: 0 6px 8px rgba(0, 0, 0, 0.2);
}

.kcButtonClass:active {
    transform: translateY(0);
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}

.kcButtonClass:focus {
    outline: none;
    box-shadow: 0 0 0 3px rgba(37, 117, 252, 0.5);
}
```
{% endcode %}

</details>

## Remove All the Default Styles

Maybe you'd prefer to remove all the Patterfly style alltogether and start fresh.

<figure><img src=".gitbook/assets/image (63).png" alt="" width="375"><figcaption><p>The login page completely unstyled (doUseDefaultCss set to false).</p></figcaption></figure>

What's nice with this approach is that not only will all the pf classes been stripped out all at once but also the global Patterfly stylesheet won't even be loaded.

{% tabs %}
{% tab title="React" %}
<pre class="language-tsx" data-title="src/login/KcPages.tsx"><code class="lang-tsx">// ...
&#x3C;DefaultPage
    kcContext={kcContext}
    i18n={i18n}
    classes={classes}
    Template={Template}
<strong>    doUseDefaultCss={false}
</strong>    UserProfileFormFields={UserProfileFormFields}
    doMakeUserConfirmPassword={doMakeUserConfirmPassword}
/>
</code></pre>
{% endtab %}

{% tab title="Svelte" %}
<pre class="language-html" data-title="src/login/KcPage.svelte"><code class="lang-html">{#await page() then { default: Page }}
    &#x3C;Page
      {kcContext}
      i18n={i18n}
      {classes}
      {Template}
      {UserProfileFormFields}
<strong>      doUseDefaultCss={false}
</strong>      {doMakeUserConfirmPassword}
    >&#x3C;/Page>
{/await}
</code></pre>


{% endtab %}

{% tab title="Angular" %}
<pre class="language-typescript" data-title="src/login/KcPage.ts"><code class="lang-typescript">const classes = {} satisfies { [key in ClassKey]?: string };
<strong>const doUseDefaultCss = false;
</strong>const doMakeUserConfirmPassword = true;

export async function getKcPage(pageId: KcContext['pageId']): Promise&#x3C;KcPage> {
  switch (pageId) {
    default:
      return {
        PageComponent: await getDefaultPageComponent(pageId),
        TemplateComponent,
        UserProfileFormFieldsComponent,
        doMakeUserConfirmPassword,
        doUseDefaultCss,
        classes,
      };
  }
}

</code></pre>


{% endtab %}
{% endtabs %}

### Disabeling the default styles only on some page.

A common scenario is to use npx keycloakify eject-page to eject some pages of the login UI to customize them in depth. &#x20;

On the page that you have ejected, you're likely to want to disable all the default styles, however you might want to keep the Patterfly styles on the pages that you haven't redesigned.\
Here is an example where the login.ftl page have been ejected, disable the default styles for it while keeping them for the other pages:

{% tabs %}
{% tab title="React" %}
<pre class="language-tsx" data-title="src/login/KcPages.tsx"><code class="lang-tsx">switch (kcContext.pageId) {
    case "login.ftl":
        return (
            &#x3C;Login
                {...{ kcContext, i18n, classes }}
                Template={Template}
<strong>                doUseDefaultCss={false}
</strong>            />
        );
    default:
        return (
            &#x3C;DefaultPage
                kcContext={kcContext}
                i18n={i18n}
                classes={classes}
                Template={Template}
<strong>                doUseDefaultCss={true}
</strong>                UserProfileFormFields={UserProfileFormFields}
                doMakeUserConfirmPassword={doMakeUserConfirmPassword}
            />
        );
}
</code></pre>


{% endtab %}

{% tab title="Angular" %}
<pre class="language-typescript" data-title="src/login/KcPage.ts"><code class="lang-typescript">  switch (pageId) {
    case 'login.ftl':
      return {
        PageComponent: (await import('./pages/login/login.component')).LoginComponent,
        TemplateComponent,
        UserProfileFormFieldsComponent,
        doMakeUserConfirmPassword,
<strong>        doUseDefaultCss: false,
</strong>        classes,
      };
    default:
      return {
        PageComponent: await getDefaultPageComponent(pageId),
        TemplateComponent,
        UserProfileFormFieldsComponent,
        doMakeUserConfirmPassword,
<strong>        doUseDefaultCss: true,
</strong>        classes,
      };
  }
</code></pre>
{% endtab %}

{% tab title="Svelte" %}
<pre class="language-html" data-title="src/login/KcPage.svelte"><code class="lang-html">&#x3C;script lang="ts">
  // ...
<strong>  const doUseDefaultCss = (()=>{
</strong><strong>    switch(kcContext.pageId){
</strong><strong>      case "login.ftl": return false;
</strong><strong>      return true;
</strong><strong>    }
</strong><strong>  })();
</strong>  
  const page = async (): Promise&#x3C;{ default?: Component&#x3C;any> }> => {
    switch (kcContext.pageId) {
      case 'login.ftl':
        return import('./pages/Login.svelte"');
      default:
        return import('@keycloakify/svelte/login/DefaultPage.svelte');
    }
  };
&#x3C;/script>

{#await page() then { default: Page }}
    &#x3C;Page
      {kcContext}
      i18n={i18n}
      {classes}
      {Template}
      {UserProfileFormFields}
<strong>      {doUseDefaultCss}
</strong>      {doMakeUserConfirmPassword}
    >&#x3C;/Page>
{/await}

</code></pre>
{% endtab %}
{% endtabs %}

### Removing the classes in the ejected components (kcClsx)

If you have ejected some pages with [`npx keycloakify eject-page`](common-use-case-examples/using-a-component-library.md) and disabled the defaut styles with `doUseDefaultCss` set to `false` you might wonder if you need to keep the `kcClsx` in the pages, for example:

<pre class="language-tsx" data-title="src/login/pages/Login.tsx"><code class="lang-tsx">&#x3C;input
    tabIndex={7}
    disabled={isLoginButtonDisabled}
<strong>    className={kcClsx(
</strong><strong>        "kcButtonClass", 
</strong><strong>        "kcButtonPrimaryClass", 
</strong><strong>        "kcButtonBlockClass", 
</strong><strong>        "kcButtonLargeClass"
</strong><strong>    )}
</strong>    name="login"
    id="kc-login"
    type="submit"
    value={msgStr("doLogIn")}
/>
</code></pre>

The answer is no, feel free to remove them! &#x20;

Just be aware that if you have defined some custom CSS that use the kc classes as selector ( Like for example `.kcButtonClass { /* ... */ }` ) they will no longer be applied.

[^1]: If you are using Angular or Svelte you lilely plan to customize the page at the component level so this isn't relevent to you.
