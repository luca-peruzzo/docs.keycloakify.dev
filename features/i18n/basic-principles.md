# Basic principles

{% hint style="info" %}
This documentation explains how i18n works in the Login theme.\
\
In the Account Multi-Page theme, everything works the same as in the Login theme. You just need to replace **/login/** by **/account/** in the import paths.

\
In the Account Single-Page theme, things works differently. [See relevent doc](https://github.com/keycloakify/docs.keycloakify.dev/blob/v11_next/features/account-theme/single-page.md#i18n-internationalization-and-translation).
{% endhint %}

In the Keycloak Admin Console you can enable localisation by selecting a set of language that you wish to support:

<figure><img src="../../.gitbook/assets/image (15).png" alt=""><figcaption><p>Enabling English, French and Spanish as supported languages in a Keycloak Realm</p></figcaption></figure>

{% hint style="info" %}
Want to add languages not in the default set into Keycloakify? [See how](adding-support-for-extra-languages.md).
{% endhint %}

When Internationalization is enabled you will see a language dropdown select in your UIs:

<figure><img src="../../.gitbook/assets/image (16).png" alt="" width="375"><figcaption></figcaption></figure>

{% hint style="success" %}
You shouldn't rely on the language select to let your users select their language.

Infact, I encourage you to hide or remove it.

What you should do instead is, when redirecting your user from your application to your Keycloak login page, add an extra query param to let Keycloak know in what language the page should be rendered.

The parameter to add is ?ui\_locales=fr (Example if we want the UI to be in French).

See [oidc-spa documentation](https://docs.oidc-spa.dev/documentation/usage) for more info on how to provide this parameter. (You can do the same if you use keycloak-js or NextAuth)
{% endhint %}

If you [eject some pages](https://github.com/keycloakify/docs.keycloakify.dev/blob/v11_next/features/customization-strategies/component-level-customization/README.md), you'll see in your component how the internationalization is actually implemented:

{% tabs %}
{% tab title="React" %}
<pre class="language-tsx" data-title="src/login/Register.tsx"><code class="lang-tsx">export default function Register(props: RegisterProps) {
    const { i18n } = props;
    
    const { msg, msgStr, advancedMsg, advancedMsgStr } = i18n;

    return (
        //...
        &#x3C;a href={url.loginUrl}>
<strong>            {msg("backToLogin")}
</strong>        &#x3C;/a>
        // ...
    );
}
</code></pre>


{% endtab %}

{% tab title="Svelte" %}
{% code title="src/login/pages/Register.svelte" %}
```html
<script lang="ts">
  // ...
  const props: RegisterProps = $props();
  const { i18n } = props;
  const { msg, msgStr, advancedMsg } = $i18n;
</script>

<a href={url.loginUrl}>{@render msg('backToLogin')()}</a>
```
{% endcode %}
{% endtab %}

{% tab title="Angular" %}
{% code title="src/login/pages/register/register.component.html" %}
```html
<a
  [href]="url.loginUrl"
  [innerHTML]="i18n.msgStr('backToLogin') | kcSanitize: 'html'"
></a>
```
{% endcode %}
{% endtab %}
{% endtabs %}



<figure><img src="../../.gitbook/assets/image (17).png" alt="" width="374"><figcaption><p><code>msg("backToLogin")</code> gets rendered as <strong>« Back to Login</strong></p></figcaption></figure>

If you want to see the base message translations you can navigate to the **node\_modules/keycloakify/src/login/i18n/messages\_defaultSet/** directory:

{% hint style="danger" %}
Don't edit this file directly, it's just for seeing what are the default set of i18n messages.
{% endhint %}

<figure><img src="../../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

As you can see, the translation message for the key `backToLogin` in English (**en.ts**) is:

`We are <strong>sorry</strong> ...`

{% tabs %}
{% tab title="React" %}
As a result calling `<p>{msg("backToLogin")}<p>` returns the following **JSX.Eement**:

```html
<p>
  <span data-kc-msg="backToLogin">We are <strong>sorry</strong> ...</span>
</p>
```

If you need to get the litteral string `"We are <strong>sorry</strong> ..."` instead of a JSX.Element you can use `msgStr("backToLogin")`.
{% endtab %}

{% tab title="Svelte" %}
As a result calling `<p>{@render msg("backToLogin")}</p>` returns the following snippet:

```html
<span data-kc-msg="backToLogin">We are <strong>sorry</strong> ...</span>
```
{% endtab %}

{% tab title="Angular" %}
As a result calling i18n.msgStr('errorTitleHtml') return the string "We are \<strong>sorry\</strong> ..." and you need to pass it sanitarized as \[innerHTML] so that "sorry" be redered in bold:

```html
    <p
        [innerHTML]="i18n.msgStr('errorTitleHtml') | kcSanitize: 'html'"
    ></p>
```

What will actually be rendered in the DOM will be:&#x20;

```html
<p>
  <span data-kc-msg="backToLogin">We are <strong>sorry</strong> ...</span>
</p>
```
{% endtab %}
{% endtabs %}

The purpose of the `data-kc-msg` attribute is to help you identify the i18n key of the text you want to change when inspecting the DOM.

<figure><img src="../../.gitbook/assets/image (19).png" alt=""><figcaption><p>Inspecting the DOM we can see that if we want to change the "Back to Login..." we need to change the "backToLogin" key.</p></figcaption></figure>

Now that you get the main idea, let's see how to preview your pages in different languages:

{% content-ref url="previewing-your-pages-in-different-languages.md" %}
[previewing-your-pages-in-different-languages.md](previewing-your-pages-in-different-languages.md)
{% endcontent-ref %}
