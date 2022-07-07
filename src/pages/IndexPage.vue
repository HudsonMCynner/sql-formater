<template>
  <q-page>
    <div class="" style="display: flex; flex-direction: column; justify-content: center; align-items: center">
      <span class="q-pa-md" style="font-size: 25px">Online SQL Editor</span>
      <p style="font-size: 16px; text-align: justify;" class="q-pa-md">
        Editar SQL online a partir de qualquer dispositivo com um navegador moderno como o Chrome e Firefox
        <br>
        Este editor on-line, devido à sua natureza baseada na Web, é absolutamente portátil e multiplataforma — você só precisa de um navegador de web usual sem plugins, e não importa em qual plataforma você está usando: PC desktop ou smartphone, Windows, Linux ou macOS, Android ou iOS.
      </p>
    </div>
    <div class="title flex flex-center q-pa-md">
      <app-sql-editor />
    </div>
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
