<template>
  <div style="height: 60vh; overflow: auto; position: relative;">
    <div ref="container" class="viewerContainer" >
      <div  class="pdfViewer" ref="viewer"></div>
    </div>
  </div>
</template>

<script>
import * as PDF from 'pdfjs-dist/legacy/build/pdf.js'
import * as PdfjsViewer from 'pdfjs-dist/legacy/web/pdf_viewer'
import viewerStyle from '!!css-loader!pdfjs-dist/legacy/web/pdf_viewer.css'
PDF.GlobalWorkerOptions.workerSrc = '/cdn/pdf.worker.min.js'

// // Setting worker path to worker bundle.
// PDF.GlobalWorkerOptions.workerSrc =
//   "./js/pdf.worker.bundle.js";

export default {
  name: 'Viewer',
  props: {
    source: {
      type: [Object, String],
      required: true
    }
  },
  data() {
    return {
      pdf: null,
      viewerContentHeight: 0,
      viewportHeight: 0,
      viewportWidth: 0,
      imageList: [],
      viewerImageList: [],
      viewerStyle: viewerStyle.toString(),
    }
  },
  watch: {
    source: {
      immediate: true,
      async handler() {
        await this.load()
        this.render()
      }
    }
  },
  activated() {
    this.render()
  },
  mounted() {

  },
  beforeDestroy() {

  },
  methods: {
    async load() {
      if (!this.source) {
        return
      }
      try {
        const filename = PDF.getFilenameFromUrl(this.source)
        console.log(PDF, '----')
        this.$emit('update:filename', filename)
        const documentLoadingTask = PDF.getDocument({
          url: this.source,
          cMapPacked: true,
          cMapUrl: 'https://unpkg.com/browse/pdfjs-dist@2.2.228/cmaps/'
        })
        documentLoadingTask.onPassword = (callback, reason) => {
          const retry = reason === PDF.PasswordResponses.INCORRECT_PASSWORD
          this.$emit('password-requested', {
            callback,
            retry
          })
        }
        this.pdf = await documentLoadingTask.promise
      } catch (e) {
        console.log(e)
        this.pdf = null
        this.$emit('loading-failed', e)
      } finally {
        console.log('--------', this.source, this.pdf)
      }
    },
    async render() {
      if (!this.pdf) {
        return
      }
      try {
        const eventBus = new PdfjsViewer.EventBus()
        this.$emit('update:isRendering', true)
        await this.$nextTick()
        const pdfViewer = new PdfjsViewer.PDFViewer({
          container: this.$refs.container,
          viewer: this.$refs.viewer,
          eventBus,
        });
        eventBus.on('pagesinit', function() {
          pdfViewer.currentScaleValue = 'page-width'
        })
        pdfViewer.setDocument(this.pdf)
      } catch (e) {
        if (e instanceof Error) {
          console.error('rendering failed:', e)
        }
        this.pdf = null
        this.$emit('rendering-failed', e)
      }
    }

  }
}

</script>
<style lang="less" src="./PdfViewer.less" scoped></style>
<style lang="css" src="pdfjs-dist/legacy/web/pdf_viewer.css"></style>