# Quasar i18n example app (quasar-i18n-example)

An app for demonstration of i18n ([Internationalization and localization](https://en.wikipedia.org/wiki/Internationalization_and_localization)) in Quasar Framework.

Please contribute more language translations!

![Building and deploying to surge](https://github.com/tomers/quasar-i18n-example/workflows/Building%20and%20deploying%20to%20surge/badge.svg)

## Demo
[Demo app](http://quasar-i18-example.surge.sh/)

## Relevant documentation
- [Quasar Language Packs](https://quasar.dev/options/quasar-language-packs)
- [App Internationalization (I18n)](https://quasar.dev/options/app-internationalization)
- [RTL Support](https://quasar.dev/options/rtl-support)
- [Best practice for presenting languages](http://www.flagsarenotlanguages.com/blog/best-practice-for-presenting-languages/)
- [International and multilingual sites](https://support.google.com/webmasters/topic/2370587)

## Integration with existing project
- Add Vue-i18n support

  Follow [Quasar's App Internationalization documentation](https://quasar.dev/options/app-internationalization). TL;DR:
  - Setup i18n
  - Build default translation file
    * Create default translation file, `src/i18n/en-us` (rename according to your project's default local, as defined in `quasar.conf.js`, in `framework.lang field`)
  - Replace existing strings with $t() macros

- Copy the `i18n.config.js` file to your project and configure it to your preferences

- Set default project language in `quasar.conf.js`

- Optionally: Enable the [Cookies plugin](https://quasar.dev/quasar-plugins/cookies#Installation), for persistency of language selection

- Add language selection logic

  - If you plan to use the LanguageSelect component:
    - Copy the `LanguageSelectMixin.vue` and `LanguageSelect.vue` files to your project
    - Add the select component anywhere in your layout

  - Otherwise:
    - Copy the `LanguageSelectMixin.vue` file to your project
    - Use the mixin in your layout

- Edit the `webpackInclude` comment in `LanguageSelectMixin.vue` according to your desired languages

## Install the dependencies
```bash
yarn
```

### Start the app in development mode (hot-code reloading, error reporting, etc.)
```bash
quasar dev
```

### Lint the files
```bash
yarn run lint
```

### Build the app for production
```bash
quasar build
```

### Customize the configuration
See [Configuring quasar.conf.js](https://quasar.dev/quasar-cli/quasar-conf-js).
