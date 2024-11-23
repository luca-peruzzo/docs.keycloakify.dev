# Integrating Keycloakify in your Codebase

{% hint style="info" %}
**Before You Start**:

This documentation section is relevant only if **you already have a project** and want to add a Keycloak theme as one of its deliverables.&#x20;

One of the powerful aspects of Keycloakify is its ability to let you reuse components and styles from your main application in your Keycloak theme. However, if you don't have an existing codebase, it’s easier to fork [the starter project](https://github.com/keycloakify/keycloakify-starter) and develop your Keycloak theme as a standalone project.
{% endhint %}

There is two main approach to integrate Keycloakify into your project, pick the one that you think will work best for you.

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><strong>React SPA</strong></td><td></td><td>If you happen to be developing a React Single Page Application with Vite or Webpack you can install Keycloakify directly within your project!</td><td><a href="in-your-react-project/">in-your-react-project</a></td></tr><tr><td><strong>Monorepo</strong></td><td><p></p><p>Use this when for example:</p><ul><li>You are using Next.js or another meta framwork that involves server side rendering.</li><li>You are using Angular Workspaces</li></ul></td><td></td><td><a href="as-a-subproject-of-your-monorepo/">as-a-subproject-of-your-monorepo</a></td></tr></tbody></table>

