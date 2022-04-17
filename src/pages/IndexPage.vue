<template>
  <q-page>
    <app-sql-editor />
  </q-page>
</template>

<script>
import { defineComponent } from 'vue'
import AppSqlEditor from 'pages/AppSqlEditor'

export default defineComponent({
  name: 'IndexPage',
  components: { AppSqlEditor }
})
</script>
