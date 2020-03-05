<template>
  <q-card>
    <q-card-section>
      <div class="text-h6">
        {{ $t('languages') }}
      </div>
    </q-card-section>

    <q-card-section>
      <q-list separator>
        <q-item
          v-for="(langOption, i) in langOptions"
          :key="`lang_${i}`"
          :active="langOption.value === lang"
          color="yellow"
          clickable
          @click="onLang(langOption.value)"
        >
          <q-item-section>
            <q-item-label>
              {{ langOption.label }}
            </q-item-label>
          </q-item-section>
          <q-item-section
            side
            top
          >
            <div class="column q-gutter-sm">
              <q-badge
                v-if="langOption.value === savedLang"
                color="secondary"
                label="Startup Cookie"
              />
              <q-badge
                v-if="langOption.value === browserLang.toLowerCase()"
                color="secondary"
                label="Browser"
              />
              <q-badge
                v-if="langOption.value === 'ru'"
                color="warning"
                label="Partial"
              />
            </div>
          </q-item-section>
        </q-item>
      </q-list>
    </q-card-section>
  </q-card>
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
    this.langOptions = this.appLanguages && this.appLanguages.map(langObj => ({
      label: langObj.nativeName, value: langObj.isoName
    }))
  },
  methods: {
    onLang (lang) {
      this.lang = lang
    }
  }
}
</script>
