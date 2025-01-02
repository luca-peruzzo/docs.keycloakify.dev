---
icon: circle-half-stroke
---

# Dark Mode Persistence

If your app features an dark/light mode you might be wondering how to "transport" the mode to your login theme so that you can ensure that if your user is brwosing your app in dark mode when they click on "Login", they get redirected to your Keycloakify login UI rendered in dark mode.

{% embed url="https://youtu.be/EbNGhg5aNF8" %}
Example of a Keycloakify theme implementation that implement dark mode persistance
{% endembed %}

This is somewhat of a niche usecase but it's interesting as it will help you understant how you can transport states from your app to your Keycloak UIs. &#x20;

## In your Web Application

So typically, from your web application, when your user click on your "Login" button in your header you will redirect it to a URL that look a bit like this: &#x20;

**https://\<your-keycloak-url>/realms/protocol/openid-connect/auth?client\_id=\<your-client>**

What we want to do is add, for example `"&dark=true"` or `"&dark=false"` to the URL so that it can be retreived on the other side by your Keycloak theme.

The way you'd do that will vary depending on your Stack but let's see an example whit:

* A React SPA
* [oidc-spa](https://www.oidc-spa.dev/): A modern alternative to keycloak-js
* [MUI](https://mui.com/material-ui/): A popular React Component library in the React world

The following snippet is a React component that you would typically use in the header of your application to display the Login and Register buttons.

<figure><img src="../.gitbook/assets/image (201).png" alt=""><figcaption></figcaption></figure>

```tsx
import { useOidc } from "oidc";
import { useTheme } from "@mui/material/styles";
import Button from "@mui/material/Button";
import { assert } from "tsafe/assert";

export function AuthButtons() {
  const { isUserLoggedIn, login } = useOidc();

  assert(!isUserLoggedIn, "If this component is rendered, the user should not be logged in");

  const theme = useTheme();

  const extraQueryParams = {
    dark: theme.palette.mode === "dark" ? "true" : "false",
    // ui_locales is a special query param that Keycloak recognize
    // you can set it to make sure that the login pages will be 
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

                urlObj.pathname = urlObj.pathname.replace(/\/auth$/, "/registrations");

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

## In your Login Theme

In your Keycloak theme now, you can create an utility that will read your cusom `&dark=true|false` parameter.

{% code title="src/shared/isDark.ts" %}
```typescript
const SESSION_STORAGE_KEY = "isDark";

function getIsDark(): boolean | undefined {
    from_url: {

        const url = new URL(window.location.href);

        const value = url.searchParams.get("dark");

        if (value === null) {
            // There was no &dark= query param in the URL
            // we will try to see if it's in the session storage.
            break from_url;
        }

        // Removing &dark= from the URL (just to be cute)
        {
            url.searchParams.delete("dark");
            window.history.replaceState({}, "", url.toString());
        }

        const isDark = value === "true";
        
        // Persisting the value in session storage so that
        // if the user navigate for example from login.ftl to
        // register.ftl we do not lose the state.
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

    return undefined;
}
```
{% endcode %}

The way you would use this utility will very much tepend of your framwork and ui library but for example with React/MUI this it what it could look like:

<pre class="language-tsx" data-title="src/login/KcPage.tsx"><code class="lang-tsx">import type { KcContext } from "./KcContext";
import { useI18n } from "./i18n";
import DefaultPage from "keycloakify/login/DefaultPage";
import Template from "keycloakify/login/Template";
<strong>import { createTheme, ThemeProvider } from "@mui/material/styles";
</strong><strong>import { getIsDark } from "../shared/isDark";
</strong>
const UserProfileFormFields = lazy(() => import("keycloakify/login/UserProfileFormFields"));

const doMakeUserConfirmPassword = true;

<strong>const theme_dark = createTheme({
</strong><strong>    palette: {
</strong><strong>        mode: "dark"
</strong><strong>    }
</strong><strong>});
</strong>
<strong>const theme_light = createTheme({
</strong><strong>    palette: {
</strong><strong>        mode: "light"
</strong><strong>    }
</strong><strong>});
</strong>
<strong>export default function KcPage(props: { kcContext: KcContext }) {
</strong><strong>    return (
</strong><strong>        &#x3C;ThemeProvider theme={getIsDark() ? theme_dark : theme_light}>
</strong><strong>            &#x3C;KcPageContextualized {...props} />
</strong><strong>        &#x3C;/ThemeProvider>
</strong><strong>    );
</strong><strong>}
</strong>
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
