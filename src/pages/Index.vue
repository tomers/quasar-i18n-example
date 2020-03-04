<template>
  <q-page class="flex flex-center">
    <div class="column q-gutter-md">
      <form-example />
      <language-list />
    </div>
  </q-page>
</template>

<script>
import FormExample from '../components/FormExample'
import LanguageList from '../components/LanguageList'

export default {
  name: 'PageIndex',
  components: {
    FormExample,
    LanguageList
  }
}
</script>
