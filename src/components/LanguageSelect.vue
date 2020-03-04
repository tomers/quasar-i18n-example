<template>
  <q-select
    v-if="lang"
    v-model="lang"
    :options="langOptions"
    bg-color="grey"
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
  watch: {
    lang () {
      // dynamic import, so loading on demand only
      import(
        /* webpackInclude: /(he|en-us)\.js$/ */
        `quasar/lang/${this.lang}`
      ).then(lang => {
        this.$q.lang.set(lang.default)
        this.$i18n.locale = lang.default.isoName
      })
    }
  },
  created () {
    // set language according to quasar's framework.lang variable, as configured
    // in quasar.conf.js
    const frameworkLang = this.$q.lang.isoName

    if (!appLanguages.map(lang => lang.isoName).includes(frameworkLang)) {
      // do not use this component when framework's language is not in the list
      // of available languages
      return
    }
    this.lang = frameworkLang

    // populate language selector
    this.langOptions = appLanguages.map(lang => ({
      label: lang.nativeName, value: lang.isoName
    }))
  }
}
</script>
