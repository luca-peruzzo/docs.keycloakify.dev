---
icon: circle-half-stroke
---

# Dark Mode Persistence

If your app offers a dark/light mode, you might be wondering how to “transfer” that mode to your login theme. This ensures that if users are browsing your app in dark mode, then click “Login,” they’ll be redirected to a Keycloakify login UI that’s rendered in dark mode.

{% embed url="https://youtu.be/EbNGhg5aNF8" %}
Example of a Keycloakify theme implementation that carries over dark mode
{% endembed %}

This is somewhat of a niche use case, but it illustrates how you can pass state from your application to your Keycloak UIs.

## In Your Web Application

Typically, when your user clicks your “Login” button in the header, your application will redirect them to a URL that looks something like this:

**https://\<your-keycloak-url>/realms/protocol/openid-connect/auth?client\_id=\<your-client>**

What we want to do is append, for example, `&dark=true` or `&dark=false` to that URL so it can be retrieved on the other side by your Keycloak theme.

How you do that depends on your stack. Let’s look at an example with:

* A React SPA
* [oidc-spa](https://www.oidc-spa.dev/), a modern alternative to `keycloak-js`
* [MUI](https://mui.com/material-ui/), a popular React component library

The following snippet is a React component typically placed in the header of your application for displaying Login and Register buttons.

<figure><img src="../.gitbook/assets/image (201).png" alt=""><figcaption></figcaption></figure>

```tsx
import { useOidc } from "oidc";
import { useTheme } from "@mui/material/styles";
import Button from "@mui/material/Button";
import { assert } from "tsafe/assert";

export function AuthButtons() {
  const { isUserLoggedIn, login } = useOidc();

  assert(
    !isUserLoggedIn,
    "If this component is rendered, the user should not be logged in"
  );

  const theme = useTheme();

  const extraQueryParams = {
    dark: theme.palette.mode === "dark" ? "true" : "false",
    // ui_locales is a special query param that Keycloak recognizes.
    // You can set it to make sure the login pages are
    // displayed in the correct language.
    ui_locales: "en"
  };

  return (
    <>
      <Button
        onClick={() =>
          login({
            doesCurrentHrefRequiresAuth: false,
            extraQueryParams
          })
        }
      >
        Login
      </Button>
      <Button
        variant="contained"
        onClick={() =>
          login({
            doesCurrentHrefRequiresAuth: false,
            transformUrlBeforeRedirect: url => {
              const urlObj = new URL(url);
              urlObj.pathname = urlObj.pathname.replace(
                /\/auth$/,
                "/registrations"
              );
              return urlObj.href;
            },
            extraQueryParams
          })
        }
      >
        Register
      </Button>
    </>
  );
}
```

## In Your Login Theme

Within your Keycloak theme, you can now create a utility to read your custom `&dark=true|false` parameter.

{% code title="src/shared/isDark.ts" %}
```typescript
const SESSION_STORAGE_KEY = "isDark";

function getIsDark(): boolean {
    from_url: {
        const url = new URL(window.location.href);

        const value = url.searchParams.get("dark");

        if (value === null) {
            // There was no &dark= query param in the URL,
            // so we check session storage next.
            break from_url;
        }

        // Remove &dark= from the URL (just to keep it clean)
        url.searchParams.delete("dark");
        window.history.replaceState({}, "", url.toString());

        const isDark = value === "true";
        
        // Persist the value in session storage so that
        // if the user navigates, for example, from login.ftl to
        // register.ftl, we don’t lose the state.
        sessionStorage.setItem(SESSION_STORAGE_KEY, `${isDark}`);

        return isDark;
    }

    from_session_storage: {
        const value = sessionStorage.getItem(SESSION_STORAGE_KEY);

        if (value === null) {
            break from_session_storage;
        }

        return value === "true";
    }

    // Return the browser preference
    return window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches;
}
```
{% endcode %}

How you use this utility depends heavily on your framework and UI library. As an example, here’s what it might look like with React/MUI:

<pre class="language-tsx" data-title="src/login/KcPage.tsx"><code class="lang-tsx">import type { KcContext } from "./KcContext";
import { useI18n } from "./i18n";
import DefaultPage from "keycloakify/login/DefaultPage";
import Template from "keycloakify/login/Template";
import { createTheme, ThemeProvider } from "@mui/material/styles";
<strong>import { getIsDark } from "../shared/isDark";
</strong>const UserProfileFormFields = lazy(() => import("keycloakify/login/UserProfileFormFields"));

const doMakeUserConfirmPassword = true;

const theme_dark = createTheme({
    palette: {
        mode: "dark"
    }
});
const theme_light = createTheme({
    palette: {
        mode: "light"
    }
});

export default function KcPage(props: { kcContext: KcContext }) {
    return (
<strong>        &#x3C;ThemeProvider theme={getIsDark() ? theme_dark : theme_light}>
</strong>            &#x3C;KcPageContextualized {...props} />
        &#x3C;/ThemeProvider>
    );
}

function KcPageContextualized(props: { kcContext: KcContext }) {
    const { kcContext } = props;
    const { i18n } = useI18n({ kcContext });

    return (
        &#x3C;Suspense>
            {(() => {
                switch (kcContext.pageId) {
                    default:
                        return (
                            &#x3C;DefaultPage
                                kcContext={kcContext}
                                i18n={i18n}
                                classes={classes}
                                Template={Template}
                                doUseDefaultCss={true}
                                UserProfileFormFields={UserProfileFormFields}
                                doMakeUserConfirmPassword={doMakeUserConfirmPassword}
                            />
                        );
                }
            })()}
        &#x3C;/Suspense>
    );
}
</code></pre>
