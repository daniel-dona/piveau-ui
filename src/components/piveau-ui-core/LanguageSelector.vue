<!-- LANGUAGE SELECTOR -->

<template>
  <select
    v-model="locale"
    class="custom-select-sm language-selector mt-1 d-none d-sm-block"
  >
    <option
      v-for="lang in Object.keys(languages)"
      :key="lang"
      :value="lang"
    >
      {{ languages[lang] }}
    </option>
  </select>
</template>

<script>
  import { has } from 'lodash';

  export default {
    name: 'LanguageSelector',
    components: {
    },
    data() {
      return {
        languages: {
          bg: 'Български (bg)',
          cs: 'čeština (cs)',
          da: 'dansk (da)',
          de: 'Deutsch (de)',
          el: 'Ελληνικά (el)',
          en: 'English (en)',
          es: 'español (es)',
          et: 'eesti (et)',
          fr: 'français (fr)',
          ga: 'Gaeilge (ga)',
          hr: 'Hrvatski (hr)',
          hu: 'Magyar (hu)',
          it: 'italiano (it)',
          lt: 'lietuvių (lt)',
          lv: 'latviešu (lv)',
          mt: 'Malti (mt)',
          nl: 'Nederlands (nl)',
          pl: 'polski (pl)',
          pt: 'português (pt)',
          ro: 'română (ro)',
          sk: 'slovenčina (sk)',
          sl: 'slovenščina (sl)',
          fi: 'suomi (fi)',
          sv: 'svenska (sv)',
          no: 'norsk (no)',
        },
      };
    },
    computed: {
      locale: {
        get() {
          return this.$i18n.locale || 'en';
        },
        set(locale) {
          this.$i18n.locale = locale;
          // Wait until router is ready before changing it
          // Necessary when routing to lazy-loaded components
          // Fixes https://gitlab.fokus.fraunhofer.de/viaduct/piveau-ui/issues/210
          this.$router.onReady(() => {
            if (locale !== this.$route.query.locale) {
              this.$router.push({ query: Object.assign({}, this.$route.query, { locale }) });
            }
          });
        },
      },
    },
    created() {
      this.initLocale();
    },
    methods: {
      has,
      initLocale() {
        this.$router.onReady(() => {
          const queryLocale = this.$route.query.locale;
          if (queryLocale) {
            this.locale = queryLocale;
          } else if (this.has(this.languages, (navigator.language.substring(0, 2)))) {
            this.locale = navigator.language.substring(0, 2);
          } else {
            this.locale = 'en';
          }
        });
      },
    },
  };
</script>

<style lang="scss" scoped>
  @import '../../../node_modules/bootstrap/scss/bootstrap.scss';
  .language-selector {
    width: 150px;
  }
</style>
