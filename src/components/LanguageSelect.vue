<template>
  <q-select
    v-if="isLangValid"
    v-model="lang"
    v-bind="{options, dark, filled, outlined, borderless, standout,
             hideBottomSpace, rounded, square, dense, itemAligned}"
    :options-dark="$q.dark.isActive"
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
  props: {
    dark: Boolean,
    filled: Boolean,
    outlined: Boolean,
    borderless: Boolean,
    standout: Boolean,
    hideBottomSpace: Boolean,
    rounded: Boolean,
    square: Boolean,
    dense: Boolean,
    itemAligned: Boolean
  },
  data () {
    return {
      options: []
    }
  },
  created () {
    // populate language selector
    this.options = this.appLanguages && this.appLanguages.map(langObj => ({
      label: langObj.nativeName, value: langObj.isoName
    }))
  }
}
</script>
