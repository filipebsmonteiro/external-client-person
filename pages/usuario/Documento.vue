<template>
  <b-card class="container mt-5">
    <form>
      <input type="file" accept="image/x-png,image/gif,image/jpeg" @change="recognize">
    </form>
    <div v-if="loadProgress > 0" class="mt-5">
      <p>Carregando Imagem:</p>
      <b-progress :value="loadProgress" :max="1" show-progress animated />
    </div>
    <div v-if="documentProgress > 0" class="mt-5">
      <p>Detectando dados:</p>
      <b-progress :value="documentProgress" :max="1" show-progress animated />
    </div>

    <pre class="m-5">{{ textResult }}</pre>

    <b-button v-if="loggerHistory.length > 0" v-b-toggle.logger-table variant="link">
      Histórico de Log
    </b-button>
    <b-collapse id="logger-table">
      <b-table
        :fields="[
          {key: 'status', label: 'Status'},
          {key: 'json', text: 'JSON'}
        ]"
        :items="loggerHistory"
      >
        <template #cell(json)="{item}">
          <pre>{{ item }}</pre>
        </template>
      </b-table>
    </b-collapse>
  </b-card>
</template>

<script>
/* eslint-disable */
import { createWorker, PSM, OEM } from 'tesseract.js'

export default {
  name: 'Documento',
  data () {
    return {
      loggerHistory: [],
      loadProgress: 0,
      documentProgress: 0,
      textResult: null
    }
  },
  methods: {
    logger (message) {
      // eslint-disable-next-line no-console
      // console.log(message)

      switch (message.status) {
        case 'loading tesseract core':
          this.loadProgress = 0.10
          break
        case 'initializing tesseract':
          this.loadProgress = 0.15
          break
        case 'initialized tesseract':
          this.loadProgress = 0.25
          break
        case 'loading language traineddata':
          this.loadProgress = 0.55
          break
        case 'loading language traineddata (from cache)':
          this.loadProgress = 0.65
          break
        case 'loaded language traineddata':
          this.loadProgress = 0.75
          break
        case 'initializing api':
          this.loadProgress = 0.85
          break
        case 'initialized api':
          this.loadProgress = 1
          break
      }

      if (message.status && message.status === 'recognizing text') {
        this.documentProgress = message.progress
      }

      this.loggerHistory = [message, ...this.loggerHistory]
    },
    async recognize (evt) {
      const worker = createWorker({
        logger: m => this.logger(m),
      })
      this.textResult = null
      const img = evt.target.files[0]
      this.loadProgress = 0.01
      this.documentProgress = 0

      await worker.load()
      await worker.loadLanguage('por')
      await worker.initialize('por', OEM.LSTM_ONLY)
      await worker.setParameters({
        tessedit_pageseg_mode: PSM.SINGLE_BLOCK,
        // tessedit_char_whitelist: '0123456789',
        // tessedit_pageseg_mode: PSM.SPARSE_TEXT,*
        // tessedit_pageseg_mode: PSM.SPARSE_TEXT_OSD,*
      })
      const { data: { text } } = await worker.recognize(img)
      this.textResult = text
      await worker.terminate()
    }
  }
}
</script>

<style lang="scss" scoped/>
