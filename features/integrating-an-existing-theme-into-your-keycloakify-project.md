---
icon: arrow-up-to-line
---

# Integrating an Existing Theme into Your Keycloakify Project

If you have already created a Keycloak theme using the native theming system, you can easily import it into your Keycloakify project.

This process is straightforward and requires no special configuration. Simply copy the source files of your existing theme and place them into the `src` directory of your Keycloakify project.

<figure><img src="../.gitbook/assets/image (202).png" alt="Native login theme in a Keycloakify project"><figcaption><p>Screenshot showing a native login theme inside a Keycloakify project</p></figcaption></figure>

Below is a live demonstration using a login theme, but the same process applies to any type of theme.

{% embed url="https://youtu.be/OFg9RIM5hSw" %}

## Theme Variants Support

If your project includes theme variants, they will also work seamlessly with your imported native theme.

You can leverage a special FreeMarker variable in your `.ftl` files to display the active theme variant dynamically:

{% code title="src/login/login.ftl" %}
```ftl
<h1>${xKeycloakify.themeName}</h1>
```
{% endcode %}

To customize translations based on the active theme variant, create property files with the following naming pattern:

```
messages/messages_<language>_override_<theme name>.properties
```

For example, if you have theme variants named `vanilla` and `chocolate`, you can override the `loginAccountTitle` message key for each variant. Here's an example project structure:

<figure><img src="../.gitbook/assets/image (203).png" alt="Theme variants: vanilla and chocolate"><figcaption><p>A project with two theme variants, "vanilla" and "chocolate," where the <code>loginAccountTitle</code> message key is overridden for each variant.</p></figcaption></figure>
