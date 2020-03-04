<template>
  <q-form
    class="q-gutter-md"
    @submit="onSubmit"
    @reset="onReset"
  >
    <q-input
      v-model="name"
      :label="$t('yourName*')"
      :hint="$t('nameAndSurname')"
      filled
      lazy-rules
      :rules="[ val => val && val.length > 0 || $t('pleaseTypeSomething')]"
    />

    <q-input
      v-model="age"
      :label="$t('yourAge*')"
      filled
      type="number"
      lazy-rules
      :rules="[
        val => val !== null && val !== '' || $t('pleaseTypeYourAge'),
        val => val > 0 && val < 100 || $t('pleaseTypeARealAge')
      ]"
    />

    <q-toggle
      v-model="accept"
      :label="$t('iAcceptTheLicenseAndTerms')"
    />

    <!-- https://quasar.dev/options/quasar-language-packs#Using-Quasar-Language-Pack-in-App-Space -->
    <div>
      <q-btn
        :label="$q.lang.label.ok"
        type="submit"
        color="primary"
      />
      <q-btn
        :label="$q.lang.label.clear"
        type="reset"
        color="primary"
        flat
        class="q-ml-sm"
      />
    </div>
  </q-form>
</template>

<script>
export default {
  data () {
    return {
      name: null,
      age: null,
      accept: false
    }
  },
  methods: {
    onSubmit () {
      if (this.accept !== true) {
        this.$q.notify({
          message: this.$t('youNeedToAcceptTheLicenseAndTermsFirst'),
          color: 'red-5',
          textColor: 'white',
          icon: 'warning'
        })
      } else {
        this.$q.notify({
          color: 'green-4',
          textColor: 'white',
          icon: 'cloud_done',
          message: 'Submitted'
        })
      }
    },
    onReset () {
      this.name = null
      this.age = null
      this.accept = false
    }
  }
}
</script>
