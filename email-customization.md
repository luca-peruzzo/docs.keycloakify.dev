---
description: Customize the default email template
---

# 📧 Email Customization

_Introduced in_ [_v4.8.0_](https://github.com/InseeFrLab/keycloakify/releases/tag/v4.8.0)

{% hint style="warning" %}
Keycloakify does not yet officially integrate a way yo customize emails with React just yet.  \
There is however a [Keycloakify plugin for React email](https://github.com/timofei-iatsenko/keycloakify-emails) that you can try out.  &#x20;
{% endhint %}

It is possible to customize the emails sent to your users to confirm their email address ect.\
Just run npx keycloakify `npx keycloakify initialize-email-theme`.

You will be prompted to select the Keycloak version you want to initialize your theme with. I recomend selecting the more resent one, even if you are currently deploying on an older Keycloak version. Everything should work. &#x20;

You can remove all the template and resource file you aren't going to customize (it will fallback to the default email theme as long as you keep a `theme.properties` with `parent=base`).\
When `npx keycloakify` (`yarn keycloak`) is run it will bundle your email theme into your `.jar` file and you will be able to select it in the Keycloak administration pages.

![Selecting your email theme in the Keycloak admin](.gitbook/assets/email.png)



{% hint style="info" %}
For theme variants read [this](theme-variants.md#email-theme).
{% endhint %}
