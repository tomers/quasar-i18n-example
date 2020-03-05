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
// Note that the order of languages is set according to index.json

export default {
  data () {
    return {
      appLanguages
    }
  },
  computed: {
    appLanguageNames () {
      return this.appLanguages && this.appLanguages.map(lang => lang.isoName)
    },
    isLangValid () {
      return this.isLangSupported(this.lang)
    },
    savedLang () {
      // last selected language in the app
      return this.$q.cookies.get(LANG_COOKIE)
    },
    browserLang () {
      // user's language preferences in the browser
      return navigator && navigator.language
    },
    lang: {
      get () {
        // project's language
        // initially set from framework.lang field in quasar.conf.js
        return this.$q.lang.isoName
      },
      set (val) {
        this.setLang(val)
      }
    }
  },
  watch: {
    lang (val) {
      // update when language is modified from outside
      this.setLang(val)
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
        this.savedLang, // (must be first)
        this.browserLang,
        this.lang
      ]
      this.lang = candidateLangs.find(this.isLangSupported)
    },
    setLang (lang) {
      // dynamic import, so loading on demand only
      import(
        // Note: you must update the magic comment here with all supported
        //       languages defined in i18n.config.js, in order for them to load
        // Reference: https://webpack.js.org/api/module-methods/#magic-comments

        /* webpackInclude: /(he|en-us|ru)\.js$/ */
        `quasar/lang/${lang}`
      )
        .then(langObj => {
          this.$q.lang.set(langObj.default)
          this.$i18n.locale = langObj.default.isoName
          this.$q.cookies.set(LANG_COOKIE, langObj.default.isoName)
        })
        .catch(error => {
          console.error(`Failed to load language ${lang}`)
          console.dir(error)
          if (this.appLanguages) {
            // set to first available language
            const preferred = this.appLanguages[0].isoName
            if (lang !== preferred) {
              this.setLang(preferred)
            }
          }
        })
    }
  }
}
</script>
