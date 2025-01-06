# Inside of Keycloak

Testing your theme in Storybook is helpful, but eventually, you'll need to test it in a real Keycloak instance before deploying it to production.

## Prerequisites

1. **Install Docker**: Ensure [Docker Desktop](https://www.docker.com/products/docker-desktop/) (or just Docker) is installed and running on your computer.
2.  **Install Maven**: Maven is required to build `.jar` files locally. Check if it's already installed by running: `mvn --version`

    If not, follow the instructions below to install Maven.

{% tabs %}
{% tab title="MacOS" %}
Using [Homebrew](https://formulae.brew.sh/formula/maven):

```bash
brew install maven
```
{% endtab %}

{% tab title="Windows" %}
On Windows, use the [Chocolatey](https://chocolatey.org/) package manager:

```bash
choco install openjdk
choco install maven
```

Or follow the [manual installation guide](https://chocolatey.org/).
{% endtab %}

{% tab title="Ubuntu/Debian" %}
```bash
sudo apt-get install maven
```
{% endtab %}

{% tab title="Fedora" %}
```bash
sudo dnf install maven
```
{% endtab %}
{% endtabs %}

## Running Keycloak in Docker

Once the prerequisites are set up, you're ready to start Keycloak. Run the following command in your Keycloakify project:

```bash
npx keycloakify start-keycloak
```

You will be prompted to select the Keycloak version to spin up:

<figure><img src="../.gitbook/assets/image (198).png" alt=""><figcaption></figcaption></figure>

Keycloakify will launch the selected Keycloak version in a Docker container.

Once the container is running, you’ll see two links:

<figure><img src="../.gitbook/assets/image (199).png" alt=""><figcaption></figcaption></figure>

1. **Keycloak Admin Console**: [http://localhost:8080](http://localhost:8080)\
   The Keycloak instance is preconfigured with your theme, a custom realm named `myrealm`, and a test user (`testuser/password123`) for quick testing.\
   Changes to the configuration are saved in the `.keycloakify` directory so that they persist across restarts.
2. **Test Web App**: [https://my-theme.keycloakify.dev](https://my-theme.keycloakify.dev)\
   This app redirects to your login theme. Edits to your theme are auto-rebuilt and updated in the Keycloak container. Simply refresh the page to see changes live.

<figure><img src="../.gitbook/assets/image (183).png" alt=""><figcaption><p>Accessing the test web app and signing in with testuser/password123</p></figcaption></figure>

<details>

<summary>Inspecting `window.kcContext`</summary>

You can open your browser’s developer tools to inspect the `kcContext` object on the page. This allows you to create new stories for pages with specific configurations.

<img src="../.gitbook/assets/image (135).png" alt="" data-size="original">

</details>

Logging in with the test user redirects you to a page where you can inspect the decoded ID token (JWT) and access your custom [Account](../theme-types/account-theme/) and [Admin](../theme-types/admin-theme.md) themes (if you implement them).

<figure><img src="../.gitbook/assets/image (200).png" alt=""><figcaption></figcaption></figure>

## More Options

Want to use a custom Keycloak image? Load some extentions? Import a realm configuration?\
No problem! The start-keycloak command support many options!&#x20;

{% content-ref url="../features/compiler-options/startkeycloakoptions.md" %}
[startkeycloakoptions.md](../features/compiler-options/startkeycloakoptions.md)
{% endcontent-ref %}

