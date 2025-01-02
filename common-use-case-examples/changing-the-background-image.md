---
icon: image-landscape
---

# Changing the background image

Let's see, as an example, the different ways you have to change the backgrounds image of the login page using CSS only.

{% hint style="info" %}
There is the equivalent of this guide using CSS-in-JS [here](changing-the-background-image-css-in-js.md).
{% endhint %}

First let's [download a background image](https://coolbackgrounds.io/) an put it in our public directory:

<figure><img src="../.gitbook/assets/image (123).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
If you wish to do so, you can hot swipe assets that you have placed into your public directory in your Keycloak instance files at:

**/opt/keycloak/themes/**[**\<name of your theme>**](../features/compiler-options/themename.md)**/\<login|account>/resources/dist**

<img src="../.gitbook/assets/image (126).png" alt="" data-size="original">
{% endhint %}

Let's apply this image to the body using plain CSS

{% tabs %}
{% tab title="React" %}
{% code title="src/login/main.css" %}
```css
body.kcBodyClass {
  background: url(/background.png) no-repeat center center fixed;
}
```
{% endcode %}
{% endtab %}

{% tab title="Angular" %}
{% code title="src/styles.css" %}
```css
body.kcBodyClass {
  background: url(/background.png) no-repeat center center fixed;
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

We import the StyleSheet:

{% tabs %}
{% tab title="React" %}
{% code title="src/login/KcPage.tsx" %}
```tsx
import "./main.css";
import { Suspense, lazy } from "react";
// ...
```
{% endcode %}
{% endtab %}

{% tab title="Angular" %}
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



Result (see [testing your theme](../testing-your-theme/)):

<figure><img src="../.gitbook/assets/image (124).png" alt=""><figcaption><p>Custom background successfully applied</p></figcaption></figure>

If you prefer, you can also move the background.png image from `public/` to, for examples, `src/login/assets/background.png` and reference the image with a path relative to the CSS file, in this case it would be:

{% code title="src/login/main.css" %}
```css
body.kcBodyClass {
  background: url(./assets/background.png) no-repeat center center fixed;
}
```
{% endcode %}

In the following video I show how to load different background for different page and how to create [theme variant](../features/theme-variants.md).

{% embed url="https://youtu.be/Nkoz1iD-HOA?si=hBXt8rw72-Pvhhnr" %}
