---
icon: pen-fancy
---

# Custom Fonts

## Using a web font service

Let's see how to use, for example, [Playwrite Netherland](https://fonts.google.com/specimen/Playwrite+NL) via Google Fonts.\
\
Create the following CSS file:

{% code title="src/login/main.css" %}
```css
@import url('https://fonts.googleapis.com/css2?family=Comic+Neue:ital,wght@0,300;0,400;0,700;1,300;1,400;1,700&family=Playwrite+NL:wght@100..400&family=Playwrite+PL:wght@100..400&display=swap');

.kcHeaderWrapperClass {
    /* NOTE: We would use `body {` if we'd like the font to be applied to everything. */
    font-family: "Playwrite NL", cursive;
}
```
{% endcode %}

Then import it as a global stylesheet:

{% tabs %}
{% tab title="React" %}
<pre class="language-tsx" data-title="src/login/KcApp.tsx"><code class="lang-tsx"><strong>import "./main.css";
</strong>// ...
</code></pre>
{% endtab %}

{% tab title="Angular" %}
<pre class="language-tsx" data-title="src/login/KcApp.ts"><code class="lang-tsx"><strong>import "./main.css";
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

That's it!

<figure><img src="../.gitbook/assets/image (92).png" alt="" width="375"><figcaption><p>Playwrite NL successfully applied to the header</p></figcaption></figure>

## Using self hosted fonts

Keycloak is often used in enterprise internal network with strict network traffic control. In this context, using a Font CDN isn't an option, you want the font to be bundled in your JAR and served directly by the Keycloak server.

Let's see how we would use a self hosted copy [Vercel's Geist](https://vercel.com/font) font.

First let's download and extract [the font files](https://github.com/keycloakify/keycloakify/releases/download/v0.0.1/geist.zip) in `src/login/assets/fonts/geist/`:

<figure><img src="../.gitbook/assets/image (84).png" alt="" width="375"><figcaption></figcaption></figure>

Now let's set Geist as the default font.

Create the following CSS file:

{% code title="src/login/main.css" %}
```css
@import url(./assets/fonts/geist/main.css);

body {
  font-family: Geist;
}
```
{% endcode %}

Then import it as a global stylesheet:

{% tabs %}
{% tab title="React" %}
<pre class="language-tsx" data-title="src/login/KcApp.tsx"><code class="lang-tsx"><strong>import "./main.css";
</strong>// ...
</code></pre>
{% endtab %}

{% tab title="Angular" %}
<pre class="language-tsx" data-title="src/login/KcApp.ts"><code class="lang-tsx"><strong>import "./main.css";
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

That's it!

<figure><img src="../.gitbook/assets/image (121).png" alt=""><figcaption><p>Geist successfully applied</p></figcaption></figure>
