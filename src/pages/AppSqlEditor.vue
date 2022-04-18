<template>
  <div class="sql-editor-container">
    <div
      class="actions-editor"
    >
      <q-btn
        icon="fas fa-indent"
        @click="formatSQL"
        outline
        size="sm"
      >
        <q-tooltip>Formatar</q-tooltip>
      </q-btn>
      <q-btn
        icon="fas fa-save"
        @click="saveFile"
        outline
        size="sm"
      >
        <q-tooltip>Salvar</q-tooltip>
      </q-btn>
      <q-btn
        icon="fas fa-folder-open"
        @click="loadFile"
        outline
        size="sm"
      >
        <q-tooltip>Carregar Arquivo</q-tooltip>
      </q-btn>
      <q-btn
        icon="fas fa-trash-alt"
        @click="model = ''"
        outline
        size="sm"
      >
        <q-tooltip>Limpar</q-tooltip>
      </q-btn>
    </div>
    <div class="group-editor">
      <div
        class="sql-editor">
        <codemirror
          border
          v-model:value="model"
          :options="cmOption"
          @blur="blur"
          @input="inputSQL"
          @cursorActivity="cursorActivity"
        />
      </div>
      <div
        class="sql-editor">
        <codemirror
          border
          v-model:value="formatedSql"
          :options="cmOption"
          @blur="blur"
          @input="inputSQL"
          @cursorActivity="cursorActivity"
        />
      </div>
    </div>
  </div>
</template>

<script>
import codemirror from 'codemirror-editor-vue3'
// language
import 'codemirror/mode/sql/sql.js'
// theme
import 'codemirror/theme/idea.css'

import sqlFormatter from '@sqltools/formatter'
export default {
  name: 'AppSqlEditor',
  components: {
    codemirror
  },
  data: () => ({
    model: 'select * from',
    formatedSql: '',
    cmOption: {
      tabSize: 5,
      styleActiveLine: true,
      lineNumbers: true,
      line: true,
      mode: 'text/x-pgsql',
      theme: 'idea',
      collapseIdentical: false,
      highlightDifferences: true
    }
  }),
  methods: {
    formatSQL () {
      this.formatedSql = sqlFormatter.format(this.model, {
        language: 'sql',
        indent: '   ',
        reservedWordCase: 'upper'
      })
    },
    saveFile () {
      this.formatSQL()
      const element = document.createElement('a')
      element.setAttribute('href', 'data:text/plain;charset=utf-8,' + encodeURIComponent(this.formatedSql))
      element.setAttribute('download', 'script.sql')
      element.style.display = 'none'
      document.body.appendChild(element)
      element.click()
      document.body.removeChild(element)
    },
    loadFile () {
      const $this = this
      const element = document.createElement('input')
      element.setAttribute('type', 'file')
      element.setAttribute('id', 'tempFile')
      element.setAttribute('accept', '.txt,.sql')
      element.style.display = 'none'
      document.body.appendChild(element)
      element.click()
      element.addEventListener('input', function (event) {
        const file = event.target.files[0]
        if (file.name.match(/^.*\.(sql|txt)$/)) {
          const reader = new FileReader()
          reader.onload = function (render) {
            $this.model = render.target.result.toLowerCase()
            document.body.removeChild(element)
          }
          return reader.readAsText(file)
        }
        $this.modeel = 'Arquivo não é compativo'
        document.body.removeChild(element)
      })
    },
    cursorActivity (e) {
      this.emit('text:selected', e.doc.getSelection())
    },
    blur () {
    },
    inputSQL (event) {
      this.emit('input', event)
    },
    emit (eventName, value) {
      this.$emit(eventName, value)
    }
  }
}
</script>

<style scoped lang="sass">
.sql-editor-container
  border: 1px solid #959595
  border-radius: 3px
  height: calc(100vh - 50px)
  .actions-editor
    padding: 5px
    border-width: 1px 1px 0 1px
    border-style: solid
    border-color: #959595
    display: grid
    grid-template-columns: repeat(4, 50px)
    grid-gap: 10px
    justify-content: center
    .q-btn
      font-size: 12px
  .group-editor
    display: grid
    grid-template-columns: repeat(2, 1fr)
    grid-column-gap: 25px
    height: calc(100% - 40px)
    .sql-editor
      font-size: 16px
      height: 100%
      width: 100%
      .codemirror-container
        height: calc(100% - 7px)
      & > *
        font-size: 16px
      //height calc(100% - 30px)
      //height: calc(100vh - 255px);
      overflow: auto
      :deep(.vue-codemirror)
        border: 1px solid #959595
        height: 100%
        .CodeMirror
          height: 100%
</style>
