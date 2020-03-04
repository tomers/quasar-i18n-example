<template>
  <q-select
    v-if="isLangValid"
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
import LanguageSelectMixin from './LanguageSelectMixin'

export default {
  mixins: [
    LanguageSelectMixin
  ],
  data () {
    return {
      langOptions: []
    }
  },
  created () {
    // populate language selector
    this.langOptions = this.appLanguages && this.appLanguages.map(langObj => ({
      label: langObj.nativeName, value: langObj.isoName
    }))
  }
}
</script>
