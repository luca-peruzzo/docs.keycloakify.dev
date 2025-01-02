# accountThemeImplementation

Possible values: `"none" | "Single-Page" | "Multi-Page"`

This option is set automatically for you when you run the `npx keycloakify initialize-account-theme` command. **Do not edit the vallue manually!**

You might wonder why this option exists when it could be possible to infer it from the source code.\
The reason is that the available [keycloakVersionTargets](keycloakversiontargets.md) changes when you implement a Multi-Page account theme and when you don't. We want you to get a type error if the version target you specified are incorrect.

{% tabs %}
{% tab title="Vite" %}
<pre class="language-typescript" data-title="vite.config.ts"><code class="lang-typescript">import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import { keycloakify } from "keycloakify/vite-plugin";

export default defineConfig({
  plugins: [
    react(), 
    keycloakify({
<strong>      accountThemeImplementation: "none"
</strong>    })
  ],
})
</code></pre>
{% endtab %}

{% tab title="Webpack" %}
{% code title="package.json" %}
```json
{
  "keycloakify": {
    "accountThemeImplementation": "none"
  }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

