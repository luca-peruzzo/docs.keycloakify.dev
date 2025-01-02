---
icon: vial
---

# Testing your Theme

Two important aspects for developing a good theme are how quickly you can see your changes on the screen and how easy it is to replicate a production environment locally.

Keycloakify provides two ways to test your theme:

## Testing Outside of Keycloak

Keycloakify enables you to test your theme without requiring a real Keycloak instance to be involves.&#x20;

This approach let you preview the pages in different configurations using mock data.

This is great for getting realtime feedback when designing your page and having a quick overview of how your theme looks like as a whole.

{% content-ref url="outside-of-keycloak.md" %}
[outside-of-keycloak.md](outside-of-keycloak.md)
{% endcontent-ref %}

## Testing Inside of Keycloak

Previewing your theme with mocks is great but at some point you have to make sure that everything works as expected in real environement.

Keycloakify let you with a simple command spin up a preconfigured Keyclaok instance in a Docker container to test your theme in real conditions.

{% content-ref url="inside-of-keycloak.md" %}
[inside-of-keycloak.md](inside-of-keycloak.md)
{% endcontent-ref %}
