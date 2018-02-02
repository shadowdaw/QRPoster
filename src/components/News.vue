<template>
  <div class="newstemp">
    <div class="tempbody">
      <uploader @uploaded="modifyPoster">
        <div :style="`background-image:url(${imgsrc})`" class="poster"></div>
      </uploader>
      <div class="box">
        <div class="content">
          <h1 class="title" contenteditable="true" v-text="title"></h1>
          <p class="summary" contenteditable="true" v-text="summary"></p>
        </div>
      </div>
    </div>
    <div class="footer">
      <uploader @uploaded="modifyQrcode">
        <img :src="currentQrcodeSrc" alt="" class="qrcode">
      </uploader>
      <div class="info">
        <p contenteditable="true">长按二维码查看原文</p>
      </div>
    </div>
  </div>
</template>
<script>
import Uploader from '@/components/FileUp2Base64';
export default {
  name: 'News',
  data: function() {
    return {
      qrurl: '',
      title: '点击编辑新闻标题',
      summary: '点击编辑新闻摘要',
      imgsrc: '/static/poster.jpg',
      qrposter: '',
      currentQrcodeSrc: this.qrcodeSrc,
      site: window.location.origin
    }
  },
  props: {
    qrcodeSrc: {
      type: String,
      require: true
    }
  },
  watch: {
    'qrcodeSrc': function(v) {
      this.currentQrcodeSrc = v;
    }
  },
  methods: {
    modifyPoster: function(imgData) {
      this.imgsrc = imgData
    },
    modifyQrcode: function(imgData) {
      this.currentQrcodeSrc = imgData
    }
  },
  components: { Uploader }
}

</script>
<style scoped>
.newstemp {
  padding: 18px 12px;
  background-color: #f4f4f4;
}

.tempbody {
  background-color: #FFF;
  border-radius: 10px;
  overflow: hidden;
}

.poster {
  width: 100%;
  height: 200px;
  background-size: cover;
  background-position: center;
}

.content {
  padding: 8px 6px;
}

.title {
  text-align: center; 
  font-weight: bolder;
  font-size: 18px;
}

.summary {
  text-align: left; 
  margin: 12px 0;
  font-size: 14px;
  word-wrap: break-word;
}

.footer {
  padding: 8px;
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.qrcode {
  width: 80px;
  height: 80px;
  padding: 6px;
}

.info {
  padding: 0 12px;
  color: #999;
}

</style>
