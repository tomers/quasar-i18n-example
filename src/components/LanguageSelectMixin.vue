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
      appLanguages,
      lang: null
    }
  },
  computed: {
    appLanguageNames () {
      return this.appLanguages && this.appLanguages.map(lang => lang.isoName)
    },
    isLangValid () {
      return this.isLangSupported(this.lang)
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
          if (this.appLanguages) {
            // set to first available language
            this.lang = this.appLanguages[0].isoName
          }
        })
    }
  },
  created () {
    this.setInitialLang()
  },
  methods: {
    isLangSupported (lang) {
      return lang && this.appLanguageNames &&
        this.appLanguageNames.includes(lang.toLowerCase())
    },
    setInitialLang () {
      // find first supported language by candidate's priority
      const candidateLangs = [
        this.$q.cookies.get(LANG_COOKIE), // saved in cookie (must be first)
        navigator && navigator.language, // browser's locale
        this.$q.lang.isoName // quasar.conf.js's framework.lang field
      ]
      this.lang = candidateLangs.find(this.isLangSupported)
    }
  }
}
</script>
