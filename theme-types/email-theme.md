---
icon: envelope
---

# Email Theme

There are two ways you can create a Keycloak Email theme with Keycloakify

## Using jsx-email

{% hint style="warning" %}
This approach only works in Vite project. So not with Webpack/Create-React-App
{% endhint %}

[keycloakify-email](https://github.com/timofei-iatsenko/keycloakify-emails) is a Keycloakify plugin that enable to create an email theme using [jsx-email](https://jsx.email/). &#x20;

{% embed url="https://github.com/timofei-iatsenko/keycloakify-emails" %}

_This plugin will evenutally be integrated to Keycloakify core._  \
\
For more instruction on how to configure Keycloak to send emails [see this video](https://www.youtube.com/watch?v=IZ9LSLfWxqo\&t=177s).

## Unsing FreeMarker

`npx keycloakify initialize-email-theme`, select the `native` option.

Running this command will initialize a native email theme in the `src/email` directory.

{% embed url="https://youtu.be/IZ9LSLfWxqo" %}
