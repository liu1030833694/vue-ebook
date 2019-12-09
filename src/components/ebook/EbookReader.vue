<template>
    <div class="ebook-reader">
      <div id="read"></div>
    </div>
</template>

<script>
import Epub from 'epubjs'
import { mapActions } from 'vuex'
import { ebookMixin } from '../../utils/mixin'

global.ePub = Epub
export default {
  name: 'EbookReader',
  mixins: [ebookMixin],
  methods: {
    ...mapActions({
      _setMenuVisible: 'setMenuVisible',
      _setFileName: 'setFileName'
    }),
    prevPage () {
      if (this.rendition) {
        this.rendition.prev()
        this.hideTitleAndMenu()
      }
    },
    nextPage () {
      if (this.rendition) {
        this.rendition.next()
        this.hideTitleAndMenu()
      }
    },
    toggleTitleAndMenu () {
      this._setMenuVisible(!this.menuVisible)
    },
    hideTitleAndMenu () {
      // this.$store.dispatch('setMenuVisible', !this.menuVisible)
      this._setMenuVisible(false)
    },
    initEpub () {
      const url = 'http://localhost:8081/epub/' + this.fileName + '.epub'
      this.book = new Epub(url)
      this.rendition = this.book.renderTo('read', {
        width: innerWidth,
        height: innerHeight
        // method: 'default'
      })
      this.rendition.display()
      this.rendition.on('touchstart', event => {
        this.touchStartX = event.changedTouches[0].clientX
        this.touchStartTime = event.timeStamp
      })
      this.rendition.on('touchend', event => {
        const offsetX = event.changedTouches[0].clientX - this.touchStartX
        const time = event.timeStamp - this.touchStartTime
        if (time < 500 && offsetX > 40) {
          this.prevPage()
        } else if (time < 500 && offsetX < -40) {
          this.nextPage()
        } else {
          this.toggleTitleAndMenu()
        }
        event.stopPropagation()
      })
    }
  },
  mounted () {
    this._setFileName(this.$route.params.fileName.split('|').join('/')).then(() => {
      this.initEpub()
    })
  }
}
</script>

<style lang="scss" scoped>
@import "../../assets/styles/global.scss";
</style>
