<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="leftDrawerOpen = !leftDrawerOpen"
        />

        <q-toolbar-title>
          {{ $t('quasarApp') }}
        </q-toolbar-title>

        <language-select dark />
        <div class="q-ml-md">
          {{ $t('quasar') }} v{{ $q.version }}
        </div>
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      bordered
      content-class="bg-grey-1"
    >
      <q-list>
        <q-item-label
          header
          class="text-grey-8"
        >
          {{ $t('essentialLinks') }}
        </q-item-label>
        <EssentialLink
          v-for="link in essentialLinks"
          :key="link.title"
          v-bind="link"
        />
        <q-separator />
        <q-item>
          <q-item-section>
            <language-select borderless />
          </q-item-section>
        </q-item>
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import EssentialLink from 'components/EssentialLink'
import LanguageSelect from 'components/LanguageSelect'

export default {
  name: 'MainLayout',

  components: {
    EssentialLink,
    LanguageSelect
  },

  data () {
    return {
      leftDrawerOpen: false
    }
  },
  computed: {
    essentialLinks () {
      return [
        {
          title: this.$t('framework'),
          caption: 'quasar.dev',
          icon: 'school',
          link: 'https://quasar.dev'
        },
        {
          title: this.$t('github'),
          caption: 'tomers/quasar-i18n-example',
          icon: 'code',
          link: 'https://github.com/tomers/quasar-i18n-example'
        },
        {
          title: this.$t('demoSite'),
          caption: 'https://quasar-i18-example.surge.sh',
          icon: 'open_in_browser',
          link: 'https://quasar-i18-example.surge.sh/'
        }
      ]
    }
  }
}
</script>
