---
icon: puzzle
---

# Integrating Keycloakify in your Codebase

{% hint style="info" %}
**Before You Start**:

This documentation section is intended for cases where **you already have an existing project** and want to add a Keycloak theme as part of its deliverables.

One of Keycloakify’s strengths is its ability to let you reuse components and styles from your main application in your Keycloak theme. However, if you don’t have an existing codebase, it’s easier to [fork one of the starter projects](#user-content-fn-1)[^1] and develop your Keycloak theme as a standalone project.
{% endhint %}

There are two main approaches to integrate Keycloakify into your project.

## First Option: Installing Keycloakify Directly in Your SPA

If you are developing a [Single Page Application (SPA)](#user-content-fn-2)[^2], you can install Keycloakify directly within your project.

The main advantage of this approach is that your theme's source files will reside inside `src/keycloak-theme`, allowing you to directly import and reuse the components from your existing codebase.

This approach is suitable if your project falls into one of the following categories:

* Vite + React
* Create-React-App
* [Vite + Svelte](#user-content-fn-3)[^3]
* Webpack + React ⚠[^4]

Follow the guide corresponding to your setup:

{% content-ref url="vite.md" %}
[vite.md](vite.md)
{% endcontent-ref %}

{% content-ref url="webpack.md" %}
[webpack.md](webpack.md)
{% endcontent-ref %}

## Second Option: Setting Up Keycloakify as a Subproject in Your Monorepo

If your project is structured as a monorepo, you can add your Keycloak theme as a subproject, typically located at `apps/keycloak-theme`.

Choose the guide that matches your monorepo setup:

{% content-ref url="package-manager-workspaces.md" %}
[package-manager-workspaces.md](package-manager-workspaces.md)
{% endcontent-ref %}

{% content-ref url="turborepo.md" %}
[turborepo.md](turborepo.md)
{% endcontent-ref %}

{% content-ref url="nx.md" %}
[nx.md](nx.md)
{% endcontent-ref %}

{% content-ref url="angular-workspace.md" %}
[angular-workspace.md](angular-workspace.md)
{% endcontent-ref %}

[^1]: Starter projects:\
    React: [https://github.com/keycloakify/keycloakify-starter](https://github.com/keycloakify/keycloakify-starter)\
    Angular: [https://github.com/keycloakify/keycloakify-starter-angular-vite](https://github.com/keycloakify/keycloakify-starter-angular-vite)\
    Svelte: [https://github.com/keycloakify/keycloakify-starter-svelte](https://github.com/keycloakify/keycloakify-starter-svelte)

[^2]: If your project is build with Next.js or Remix it does not fall under this cathegory.

[^3]: Svelte projects initialized with SvelteKit 1.0.0 or later use Vite as the default build tool. If your project was created in 2023 or later, it is likely based on Vite.

[^4]: Requires some advanced configuration
