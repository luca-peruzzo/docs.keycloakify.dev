---
hidden: true
---

# Angular Workspace

{% hint style="info" %}
**Before You Start**:

This documentation section is intended for cases where **you already have an existing project** and want to add a Keycloak theme as part of its deliverables.

One of Keycloakify’s strengths is its ability to let you reuse components and styles from your main application in your Keycloak theme. However, if you don’t have an existing codebase, it’s easier to [fork one of the starter projects](https://github.com/keycloakify/keycloakify-starter-angular-vite) and develop your Keycloak theme as a standalone project.
{% endhint %}

## Integrating Keycloakify into an Angular Workspace

Let's assume you have a monorepo project where sub applications are stored in the **projects/** directory.

Next up you want to repatriate the Keycloakify Starter template sources.

```bash
cd my-workspace
yarn ng generate application keycloak-theme
rm -rf projects/keycloak-theme/public
rm -rf projects/keycloak-theme/src
git clone https://github.com/keycloakify/keycloakify-starter-angular-vite tmp
mv tmp/src projects/keycloak-theme
mv tmp/public projects/keycloak-theme
mv tmp/vite.config.ts projects/keycloak-theme
mv tmp/index.html projects/keycloak-theme
rm -rf tmp
```

#### Adjust the `vite.config.ts` File

Edit the highlighted path in `vite.config.ts` file to match the following configuration:

<pre class="language-javascript" data-title="vite.config.ts"><code class="lang-javascript">/// <reference types="vitest" />

import { defineConfig } from 'vite';
import angular from '@analogjs/vite-plugin-angular';
import { keycloakify } from 'keycloakify/vite-plugin';

// https://vitejs.dev/config/
export default defineConfig(({ mode }) => ({
  build: {
    target: ['es2022'],
  },
  <strong>root: 'projects/keycloak-theme',</strong>
  resolve: {
    mainFields: ['module'],
  },
  plugins: [
    angular(),
    keycloakify({
      accountThemeImplementation: 'none',
      <strong>themeName: 'keycloak-theme',
      keycloakifyBuildDirPath: '../../dist/keycloak-theme'</strong>
    }),
  ],
  define: {
    'import.meta.vitest': mode !== 'production',
  },
}));

</code></pre>

after this, your project structure should look like the following:

```
my-workspace/

│
├── projects/
│   └── keycloak-theme/
│       ├── src/
│       ├── public/
│       ├── index.html
│       ├── tsconfig.app.json
│       ├── tsconfig.spec.json
│       └── vite.config.ts
│
├── angular.json
└── package.json

```

### Update `package.json` with Keycloakify Configuration

Ensure the following lines are present in your workspace's `package.json`:

<pre class="language-json" data-title="package.json"><code class="lang-json">{
  "name": "my-workspace",
  "version": "0.0.0",
<strong>  "type": "module",
</strong>  "scripts": {
    ...
<strong>    "build-keycloak-theme": "ng build --project keycloak-theme &#x26;&#x26; npx keycloakify build --project projects/keycloak-theme",
</strong>    ...
  },
</strong>  "dependencies": {
    ...
<strong>    "@keycloakify/angular": "^0.2.14",
    "keycloakify": "^11.8.1",
    "marked": "^5.0.2",
    "marked-gfm-heading-id": "^3.0.4",
    "marked-highlight": "^2.0.1",
    "marked-mangle": "^1.1.7",
    "prismjs": "^1.29.0",
</strong>    ...
  },
  "devDependencies": {
    ...
<strong>    "@analogjs/platform": "^1.11.0",
    "@analogjs/vite-plugin-angular": "^1.9.0",
    "@analogjs/vitest-angular": "^1.9.0",
    "@angular-devkit/architect": "^0.1900.6",
    "@nx/angular": "^20.3.0",
    "@nx/devkit": "^20.3.0",
    "@nx/vite": "^20.3.0",
    "vite": "^5.0.0",
    "vite-tsconfig-paths": "^4.2.0",
    "vitest": "^2.0.0"
</strong>    ...
  }
}
</code></pre>

### Add the Keycloakify Project to `angular.json`

To integrate the **Keycloakify** project into your workspace, update the `angular.json` file by adding an entry to the `projects` section. Below is an example configuration. Important lines that may require customization based on your project’s requirements are highlighted:

<pre class="language-json" data-title="angular.json"><code class="lang-json">  ...
    "keycloak-theme": {
      "projectType": "application",
      "schematics": {},
      <strong>"root": "projects/keycloak-theme",
      "sourceRoot": "projects/keycloak-theme/src",
      "prefix": "kc",</strong>
      "architect": {
        "build": {
          <strong>"builder": "@analogjs/platform:vite",</strong>
          "options": {
            <strong>"configFile": "projects/keycloak-theme/vite.config.ts",
            "outputPath": "projects/keycloak-theme/dist",
            "main": "projects/keycloak-theme/src/main.ts",
            "index": "projects/keycloak-theme/index.html",
            "tsConfig": "projects/keycloak-theme/tsconfig.app.json"</strong>
          },
          "configurations": {
            "production": {
              "budgets": [
                {
                  "type": "initial",
                  "maximumWarning": "500kB",
                  "maximumError": "1MB"
                },
                {
                  "type": "anyComponentStyle",
                  "maximumWarning": "4kB",
                  "maximumError": "8kB"
                }
              ],
              "outputHashing": "all"
            },
            "development": {
              "optimization": false,
              "extractLicenses": false,
              "sourceMap": true
            }
          },
          "defaultConfiguration": "production"
        },
        "serve": {
          <strong>"builder": "@analogjs/platform:vite-dev-server",</strong>
          "configurations": {
            "production": {
              "buildTarget": "keycloak-theme:build:production",
              <strong>"port": 5173</strong>
            },
            "development": {
              "buildTarget": "keycloak-theme:build:development",
              "hmr": true
            }
          },
          "defaultConfiguration": "development"
        },
        "lint": {
          "builder": "@angular-eslint/builder:lint",
          "options": {
            "lintFilePatterns": ["src/**/*.ts", "src/**/*.html"]
          }
        }
      }
    }
</code></pre>

## Use Keycloakify

The application should now be good to go. Make sure that whenever you run a `npx keycloakify` command in your workspace root you add the path to your keycloakify project like this:\
`npx keycloakify build --project projects/keycloak-theme`
