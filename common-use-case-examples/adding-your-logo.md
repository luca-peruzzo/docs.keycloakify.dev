---
icon: file-png
description: Practical example of how to import custom assets in ejected components.
---

# Adding your Logo

{% hint style="info" %}
NOTE: You can very well change the logo using only CSS without having to ejecting the template. There's a demo in [this video](https://youtu.be/Nkoz1iD-HOA?si=6DLF7iAPTeX-pkNP). &#x20;
{% endhint %}

Let's say you want to put the logo of your company on every pages of the theme.

First you'd eject the Template:

```bash
npx keycloakify eject-page # Select login -> Template.tsx
```

<figure><img src="../.gitbook/assets/image (68).png" alt=""><figcaption></figcaption></figure>

This will create a src/login/Template.tsx file in your project.

## Import from the public directory

Let's use this placeholder for the demo: [logo.png](https://github.com/keycloakify/keycloakify/releases/download/v0.0.1/logo.png).

We put the file in public/img/logo.png

<div align="center" data-full-width="false"><figure><img src="../.gitbook/assets/image (69).png" alt="" width="563"><figcaption></figcaption></figure></div>

Now let's edit the template to import the file:

{% tabs %}
{% tab title="React - Vite" %}
It's important that you do not simply harcode `src="/img/logo.png"`, or keycloakify won't be able to patch the URL for Keycloak. Use Vite's builtin `import.meta.env.BASE_URL`.

<pre class="language-tsx" data-title="src/login/Template.tsx"><code class="lang-tsx">&#x3C;div className={kcClsx("kcLoginClass")}>
    &#x3C;div id="kc-header" className={kcClsx("kcHeaderClass")}>
        &#x3C;div id="kc-header-wrapper" className={kcClsx("kcHeaderWrapperClass")}>
<strong>            {/*{msg("loginTitleHtml", realm.displayNameHtml)}*/}
</strong><strong>            &#x3C;img src={`${import.meta.env.BASE_URL}img/logo.png`} width={500}/>
</strong>        &#x3C;/div>
    &#x3C;/div>
    {/* ... */}
</code></pre>

Doing this is a good practice in any Vite project (not specially Keycloakify) since it ensure the correctness of your URLs even if you customize the "base" parameter in the your vite.config.ts. Writing "/img/logo.png" only works when base is "/" (which is the default)
{% endtab %}

{% tab title="Svelte" %}
It's important that you do not simply harcode `src="/img/logo.png"`, or keycloakify won't be able to patch the URL for Keycloak. Use Vite's builtin `import.meta.env.BASE_URL`.

{% code title="src/login/Template.svelte" %}
```html
<div
  id="kc-header-wrapper"
  class={kcClsx('kcHeaderWrapperClass')}
>
  <!--{ msgStr('loginTitleHtml', realm.displayNameHtml) }-->
  <img src={`${import.meta.env.BASE_URL}img/logo.png`} width={500} />
</div>
```
{% endcode %}

Doing this is a good practice in any Vite project (not specially Keycloakify) since it ensure the correctness of your URLs even if you customize the "base" parameter in the your vite.config.ts. Writing "/img/logo.png" only works when base is "/" (which is the default)
{% endtab %}

{% tab title="Angular - Vite" %}
<pre class="language-typescript" data-title="src/login/template/template.component.ts"><code class="lang-typescript">export class TemplateComponent extends ComponentReference {
<strong>  BASE_URL = import.meta.env.BASE_URL;
</strong></code></pre>

{% code title="src/login/template/template.component.html" %}
```html
<img
  [src]="BASE_URL + 'img/logo.png'"
  alt="logo"
  width="500"
/>
```
{% endcode %}
{% endtab %}

{% tab title="React - Webpack/CRA" %}
It's important that you do not simply harcode `src="/img/logo.png"`, or keycloakify won't be able to patch the URL for Keycloak. Use `PUBLIC_URL` instead.

<pre class="language-tsx" data-title="src/login/Template.tsx"><code class="lang-tsx">import { PUBLIC_URL } from "keycloakify/PUBLIC_URL";

&#x3C;div className={kcClsx("kcLoginClass")}>
    &#x3C;div id="kc-header" className={kcClsx("kcHeaderClass")}>
        &#x3C;div id="kc-header-wrapper" className={kcClsx("kcHeaderWrapperClass")}>
<strong>            {/*{msg("loginTitleHtml", realm.displayNameHtml)}*/}
</strong><strong>            &#x3C;img src={`${PUBLIC_URL}/img/logo.png`} width={500}/>
</strong>        &#x3C;/div>
    &#x3C;/div>
    {/* ... */}
</code></pre>

NOTE: You can see PUBLIC\_URL as an equivalent of `process.env.PUBLIC_URL` that will work inside and outside of Keycloak.
{% endtab %}
{% endtabs %}

You can see the result by running `npx keycloakify start-keycloak`

<figure><img src="../.gitbook/assets/image (70).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
If you ever need to SSH into the Keycloak server and hot swipe the image you can find it at

**/opt/keycloak/themes/**[**\<name of your theme>**](../features/compiler-options/themename.md)**/login/resources/dist/img/logo.png**

<img src="../.gitbook/assets/image (71).png" alt="" data-size="original">
{% endhint %}

## Letting the bundle handle your import

Importing your asset from the public directory has the drawback that you won't get a compilation error if you made a mistake, like for example if you rename a file and forget to update the imports.\
A nice solution for this is to let Vite or Webpack handle the import.

Let's move our logo.png to **/src/login/assets/logo.png**

<figure><img src="../.gitbook/assets/image (72).png" alt="" width="336"><figcaption></figcaption></figure>

Now let's update the imports:

{% tabs %}
{% tab title="React" %}
<pre class="language-tsx" data-title="src/login/Template.tsx"><code class="lang-tsx"><strong>import logoPngUrl from "./assets/logo.png";
</strong>// ...
&#x3C;div className={kcClsx("kcLoginClass")}>
    &#x3C;div id="kc-header" className={kcClsx("kcHeaderClass")}>
        &#x3C;div id="kc-header-wrapper" className={kcClsx("kcHeaderWrapperClass")}>
<strong>            {/*{msg("loginTitleHtml", realm.displayNameHtml)}*/}
</strong><strong>            &#x3C;img src={logoPngUrl} width={500}/>
</strong>        &#x3C;/div>
    &#x3C;/div>
    {/* ... */}
</code></pre>
{% endtab %}

{% tab title="Svelte" %}
{% code title="src/login/Template.svelte" %}
```html
<script lang="ts">
  import logoPngUrl from "./assets/logo.png";
  // ...
</script>

<div
  id="kc-header-wrapper"
  class={kcClsx('kcHeaderWrapperClass')}
>
  <!--{ msgStr('loginTitleHtml', realm.displayNameHtml) }-->
  <img src={logoPngUrl} width={500} />
</div>
```
{% endcode %}
{% endtab %}

{% tab title="Angular" %}
<pre class="language-typescript" data-title="src/login/template/template.component.ts"><code class="lang-typescript"><strong>import logoPngUrl from '../assets/logo.png';
</strong>
export class TemplateComponent extends ComponentReference {
<strong>  logoPngUrl = logoPngUrl;
</strong>  // ...
</code></pre>

<pre class="language-html" data-title="src/login/template/template.component.html"><code class="lang-html">&#x3C;img
<strong>  [src]="logoPngUrl"
</strong>  alt="logo"
  width="500"
/>
</code></pre>
{% endtab %}
{% endtabs %}

This will yield the same result except that now if you delete, move or rename the logo.png file you'll get a compilation error letting you know that you must also update your **Template.tsx** file.
