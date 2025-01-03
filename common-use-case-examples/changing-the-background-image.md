---
icon: image-landscape
---

# Changing the background image

Let's see, as an example, the different ways you have to change the backgrounds image of the login page using CSS only.

{% hint style="info" %}
There is the equivalent of this guide using CSS-in-JS [here](changing-the-background-image-css-in-js.md).
{% endhint %}

First let's [download a background image](https://coolbackgrounds.io/) an put it in **src/login/assets/background.png**.

Then let's apply it as background:

{% code title="src/login/main.css" %}
```css
body.kcBodyClass {
  background: url(./assets/background.png) no-repeat center center fixed;
}
```
{% endcode %}

We import the StyleSheet:

{% tabs %}
{% tab title="React" %}
<pre class="language-tsx" data-title="src/login/KcPage.tsx"><code class="lang-tsx"><strong>import "./main.css";
</strong>// ...
</code></pre>
{% endtab %}

{% tab title="Svelte" %}
<pre class="language-html" data-title="src/login/KcPage.svelte"><code class="lang-html">&#x3C;script lang="ts">
<strong>  import "./main.css";
</strong>  ...
</code></pre>
{% endtab %}

{% tab title="Angular - Webpack" %}
{% code title="angular.json" %}
```json
"build": {
    "builder": "@angular-builders/custom-webpack:browser",
    "options": {
        "styles": ["src/styles.css"]
    },
```
{% endcode %}
{% endtab %}
{% endtabs %}

Result:

<figure><img src="../.gitbook/assets/image (124).png" alt=""><figcaption><p>Custom background successfully applied</p></figcaption></figure>

<details>

<summary>Replacing the image without re-building the theme</summary>

If you want to be able to "hot swipe" the image, without rebuilding the theme you have to import the image from a different location. &#x20;

Place the file into **/public/background.png**.

Then, in your CSS code import the image with an absolute path:

{% code title="src/login/main.css" %}
```css
body.kcBodyClass {
  background: url(/background.png) no-repeat center center fixed;
}
```
{% endcode %}

Now if you want to replace the image directly in Keycloak you'll be able to find it at:

**/opt/keycloak/themes/**[**\<name of your theme>**](../features/compiler-options/themename.md)**/login/resources/dist/background.png**

<img src="../.gitbook/assets/image (126).png" alt="" data-size="original">



</details>

For a more advanced example, in the following video I show how to load different background for different page and how to create [theme variant](../features/theme-variants.md).

{% embed url="https://youtu.be/Nkoz1iD-HOA?si=hBXt8rw72-Pvhhnr" %}
