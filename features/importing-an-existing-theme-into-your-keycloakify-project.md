---
icon: arrow-up-to-line
---

# Importing an Existing Theme Into Your Keycloakify project

If you already have created a Keycloak theme with the native theming system you can import it into your Keycloakify project. &#x20;

Infact it's very easy, there's is no special configuration to know about, just copy the source of your existing theme and put it into your Keycloakify src directory.

<figure><img src="../.gitbook/assets/image (202).png" alt=""><figcaption><p>Screenshot showing a native login theme inside a Keycloakify project</p></figcaption></figure>

Following is a live demo using a login theme but it works the same with every type of theme.

## Theme Variants Support

If you implement theme variants, it also applies to your native theme. &#x20;

You can use a special FreeMarker variable in your .ftl file that will take the value of the select theme variant.

{% code title="src/login/login.ftl" %}
```ftl
<h1>${xKeycloakify.themeName}</h1>
```
{% endcode %}

You can also override the messages translation depending on what theme is enabled by creating property files with the following pattern:

```
messages/messages_<language>_override_<theme name>.properties
```

<figure><img src="../.gitbook/assets/image (203).png" alt=""><figcaption><p>A project where there is two theme variant defined: "vanilla" and "chocoloate" and where the loginAccountTitle message key have been overwriten for each of them.</p></figcaption></figure>

