---
icon: album-collection
---

# Differences Between Login Themes and Other Types of Themes

Here are the types of Theme that exists: &#x20;

* **Login Theme**: The UI for login and registration pages, displayed to users when they attempt to log in or sign up.
* **Account Theme**: The account management interface, where users can update their email, change their password, and manage other account settings.
* **Email Theme**: The templates used by Keycloak for automated emails, such as email confirmation or password reset notifications.
* **Admin Theme**: The Admin Console interface, used by administrators to configure Keycloak.

Most of this documentation focuses on the Login Theme.

If you opt for implementing a Multi-Page Account theme, everything still applies, the Multi-Page Account Theme works exactly like the Login Theme.

Here are the feature that applies to all theme types:

* ✅ [Testing your theme inside Keycloak](../testing-your-theme/inside-of-keycloak.md)
* ✅ [Theme Variants](../features/theme-variants.md)
* ✅ [Environnement Variables](../features/compiler-options/environmentvariables.md)

Here are the features that only apply to the Login Theme and the Multi-Page Account Theme and not to the other types of theme:

* ❌ [Testing your theme outside of Keycloak](../testing-your-theme/outside-of-keycloak.md) (npx keycloakify add-story)
* ❌ [npx keycloakify eject-page](../common-use-case-examples/using-a-component-library.md)
* ❌ [internationalization](../features/i18n/), the other type of theme handle translation in diffrent ways.

In this section you'll get all the info you need for creating other types of theme with Keycloakify:

{% content-ref url="account-theme/" %}
[account-theme](account-theme/)
{% endcontent-ref %}

{% content-ref url="email-theme.md" %}
[email-theme.md](email-theme.md)
{% endcontent-ref %}

{% content-ref url="admin-theme.md" %}
[admin-theme.md](admin-theme.md)
{% endcontent-ref %}



