---
icon: envelope
---

# Email Theme

There are two ways you can create a Keycloak Email theme with Keycloakify

## Using the keycloakify-emails plugin

{% hint style="warning" %}
This approach only works in Vite project. So not with Webpack/Create-React-App
{% endhint %}

keycloakify-email is a Keycloakify plugin that enable to create an email theme using [jsx-email](https://jsx.email/). &#x20;

This plugin will evenutally be integrated to Keycloakify core but as for now it has to be setup separately.

{% embed url="https://github.com/timofei-iatsenko/keycloakify-emails" %}

## Using FreeMarker

`npx keycloakify initialize-email-theme`, select the `native` option.

Documentation under construction.



{% hint style="info" %}
For theme variants read [this](../features/theme-variants.md#email-theme).
{% endhint %}
