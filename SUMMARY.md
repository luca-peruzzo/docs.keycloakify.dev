# Table of contents

* [👨‍💻 Quick start](README.md)
* [🧪 Testing your Theme](testing-your-theme/README.md)
  * [In Storybook](testing-your-theme/in-storybook.md)
  * [In a Keycloak Docker Container](testing-your-theme/in-a-keycloak-docker-container.md)
  * [With Vite or Webpack in dev mode](testing-your-theme/with-vite-or-webpack-in-dev-mode.md)
* [🔩 Integrating Keycloakify in your Codebase](keycloakify-in-my-codebase/README.md)
  * [In your React Project](keycloakify-in-my-codebase/in-your-react-project/README.md)
    * [In your Vite Project](keycloakify-in-my-codebase/in-your-react-project/in-your-vite-project.md)
    * [In your Webpack Project](keycloakify-in-my-codebase/in-your-react-project/in-your-webpack-project.md)
  * [As a Subproject of your Monorepo](keycloakify-in-my-codebase/as-a-subproject-of-your-monorepo/README.md)
    * [Turborepo](keycloakify-in-my-codebase/as-a-subproject-of-your-monorepo/turborepo.md)
    * [Nx Integrated Monorepo](keycloakify-in-my-codebase/as-a-subproject-of-your-monorepo/nx-integrated-monorepo.md)
    * [Package Manager Workspaces](keycloakify-in-my-codebase/as-a-subproject-of-your-monorepo/package-manager-workspaces.md)
* [🎨 Customization Strategies](customization-strategies/README.md)
  * [CSS Level Customization](customization-strategies/css-level-customization/README.md)
    * [Basic example](customization-strategies/css-level-customization/basic-example.md)
    * [Removing the default styles](customization-strategies/css-level-customization/removing-the-default-styles.md)
    * [Applying your own classes](customization-strategies/css-level-customization/applying-your-own-classes.md)
    * [Page specific styles](customization-strategies/css-level-customization/page-specific-styles.md)
    * [Using Tailwind](customization-strategies/css-level-customization/using-tailwind.md)
    * [Using custom assets](customization-strategies/css-level-customization/using-custom-assets/README.md)
      * [.css, .sass or .less](customization-strategies/css-level-customization/using-custom-assets/plain-css.md)
      * [CSS-in-JS](customization-strategies/css-level-customization/using-custom-assets/css-in-js.md)
  * [Component Level Customization](customization-strategies/component-level-customization/README.md)
    * [Using custom assets](customization-strategies/component-level-customization/in-react-components.md)
* [🖋️ Custom Fonts](custom-fonts.md)
* [🌎 Internationalization and Translations](i18n.md)
  * [Base principles](i18n/base-principles.md)
  * [Adding Support for Extra Languages](i18n/adding-support-for-extra-languages.md)
  * [Previewing you Pages In Different Languages](i18n/previewing-you-pages-in-different-languages.md)
  * [Adding New Translation Messages Or Changing The Default Ones](i18n/adding-new-translation-messages-or-changing-the-default-ones.md)
* [🎭 Theme Variants](theme-variants.md)
* [📝 Customizing the Register Page](customizing-the-register-page.md)
* [👤 Account Theme](account-theme/README.md)
  * [Single-Page](account-theme/single-page.md)
  * [Multi-Page](account-theme/multi-page.md)
* [📄 Terms and conditions](terms-and-conditions.md)
* [🖇️ Styling a Custom Page Not Included In Base Keycloak](styling-custom-extension-page.md)
* [🔧 Accessing the Server Environment Variables](environment-variables.md)
* [🎯 Targetting Specific Keycloak Versions](targeting-specific-keycloak-versions.md)
* [📧 Email Customization](email-customization.md)
* [🚛 Passing URL Parameters to your Theme](passing-url-parameters-when-redirecting-to-your-theme.md)
* [🤵 Admin theme](admin-theme.md)
* [📥 Importing the JAR of Your Theme Into Keycloak](importing-your-theme-in-keycloak.md)
* [🔛 Enabling your Theme in the Keycloak Admin Console](enabling-your-theme.md)
* [🤓 Taking ownership of the kcContext](taking-ownership-of-the-script-responsible-for-generating-the-kccontext.md)
* [📖 Configuration Options](configuration-options/README.md)
  * [--project](configuration-options/project.md)
  * [keycloakVersionTargets](configuration-options/keycloakversiontargets.md)
  * [environmentVariables](configuration-options/environmentvariables.md)
  * [themeName](configuration-options/themename.md)
  * [startKeycloakOptions](configuration-options/startkeycloakoptions.md)
  * [themeVersion](configuration-options/themeversion-1.md)
  * [postBuild](configuration-options/postbuild.md)
  * [XDG\_CACHE\_HOME](configuration-options/xdg\_cache\_home.md)
  * [kcContextExclusionsFtl](configuration-options/kccontextexclusionsftl.md)
  * [keycloakifyBuildDirPath](configuration-options/keycloakifybuilddirpath.md)
  * [groupId](configuration-options/groupid.md)
  * [artifactId](configuration-options/artifactid.md)
  * [Webpack specific options](configuration-options/webpack-specific-options/README.md)
    * [projectBuildDirPath](configuration-options/webpack-specific-options/projectbuilddirpath.md)
    * [staticDirPathInProjectBuildDirPath](configuration-options/webpack-specific-options/staticdirpathinprojectbuilddirpath.md)
    * [publicDirPath](configuration-options/webpack-specific-options/publicdirpath.md)

## FAQ & HELP

* [⬆️ Migration Guides](faq-and-help/migration-guides/README.md)
  * [⬆️ v10->v11](faq-and-help/migration-guides/v10-greater-than-v11.md)
  * [⬆️ v9 -> v10](faq-and-help/migration-guides/v9-greater-than-v10.md)
  * [⬆️ CRA -> Vite](faq-and-help/migration-guides/cra-greater-than-vite.md)
  * [⬆️ v8 -> v9](faq-and-help/migration-guides/v8-greater-than-v9.md)
  * [⬆️ v7 -> v8](faq-and-help/migration-guides/v7-greater-than-v8.md)
  * [⬆️ v6 -> v7](faq-and-help/migration-guides/v6-greater-than-v7.md)
  * [⬆️ v6.x -> v6.12](faq-and-help/migration-guides/v6.x-greater-than-v6.12.md)
  * [⬆️ v5 -> v6](faq-and-help/migration-guides/readme-1.md)
* [😞 Can't identify the page to customize?](faq-and-help/cant-identify-the-page-to-customize.md)
* [🤔 How it Works](faq-and-help/how-it-works.md)
* [😖 Some values you need are missing from in kcContext type definitions?](faq-and-help/some-values-you-need-are-missing-from-in-kccontext.md)
* [❓ Can I use it with Vue or Angular](faq-and-help/can-i-use-it-with-vue-or-angular/README.md)
  * [Angular](faq-and-help/can-i-use-it-with-vue-or-angular/angular.md)
* [⚠️ Limitations](faq-and-help/limitations.md)
* [🛑 Errors Keycloak in Logs](faq-and-help/keycloak-error-in-log.md)
* [🙋 How do I add extra pages?](faq-and-help/how-do-i-add-extra-pages.md)
* [🤓 Can I use react-hooks-form?](faq-and-help/can-i-use-react-hooks-form.md)
* [🚀 Redirecting you users to the login/register pages](faq-and-help/redirecting-you-users-to-the-login-register-pages.md)
* [💟 Contributing](faq-and-help/contributing.md)
* [🍪 Google reCaptcha and End of third-party Cookies](faq-and-help/google-recaptcha-and-end-of-third-party-cookies.md)
* [🔖 Accessing the Realm Attributes](faq-and-help/accessing-the-realm-attributes.md)

***

* [⭐ Sponsors](sponsors.md)
