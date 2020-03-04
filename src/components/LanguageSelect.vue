<template>
  <q-select
    v-if="isShown"
    v-model="lang"
    :options="langOptions"
    :options-dark="$q.dark.isActive"
    dark
    aria-label="Language"
    emit-value
    map-options
  >
    <template v-slot:prepend>
      <q-icon name="language" />
    </template>
  </q-select>
</template>

<script>
import { availableLangs } from '../../i18n.config'
import languages from 'quasar/lang/index.json'

const LANG_COOKIE = 'lang'
let appLanguages = languages
if (availableLangs) {
  appLanguages = appLanguages.filter(lang =>
    availableLangs.includes(lang.isoName)
  )
}

export default {
  data () {
    return {
      lang: null,
      langOptions: []
    }
  },
  computed: {
    isLangInAppLangs () {
      return appLanguages && this.lang &&
        appLanguages.map(lang => lang.isoName).includes(this.lang)
    },
    isShown () {
      return this.isLangInAppLangs
    }
  },
  watch: {
    lang () {
      // dynamic import, so loading on demand only
      import(
        /* webpackInclude: /(he|en-us)\.js$/ */
        `quasar/lang/${this.lang}`
      )
        .then(lang => {
          this.$q.lang.set(lang.default)
          this.$i18n.locale = lang.default.isoName
          this.$q.cookies.set(LANG_COOKIE, lang.default.isoName)
        })
        .catch(() => {
          console.error(`Failed to load language ${this.lang}`)
          if (appLanguages) {
            // set to first available language
            this.lang = appLanguages[0].isoName
          }
        })
    }
  },
  created () {
    if (!appLanguages) {
      return
    }

    // set language according to quasar's framework.lang variable, as configured
    // in quasar.conf.js
    const savedLang = this.$q.cookies.get(LANG_COOKIE)
    const frameworkLang = this.$q.lang.isoName

    this.lang = savedLang || frameworkLang

    // populate language selector
    this.langOptions = appLanguages.map(langObj => ({
      label: langObj.nativeName, value: langObj.isoName
    }))
  }
}
</script>
