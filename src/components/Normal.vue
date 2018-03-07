<template>
  <div>
    <uploader @uploaded="modifyPoster">
      <img :src="imgsrc" alt="" class="poster">
    </uploader>
    <div class="content">
      <h1 class="title" contenteditable="true" v-text="title"></h1>
      <p class="summary" contenteditable="true" v-text="summary"></p>
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
  name: 'App',
  data: function() {
    return {
      qrurl: '',
      title: '点击编辑标题',
      summary: '点击编辑摘要',
      imgsrc: '/static/poster.jpg',
      qrposter: '',
      currentQrcodeSrc: this.qrcodeSrc,
      site: window.location.origin
    }
  },
  props:{
    qrcodeSrc:{
      type:String,
      require:true
    }
  },
  watch: {
    'qrcodeSrc':function(v){
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
.poster {
  width: 100%;
}

.content {
  padding: 10px 20px;
  text-align: left;
  border-bottom: 1px solid #EEE;
}

.title {
  font-weight: bolder;
  font-size: 18px;
}

.summary {
  margin: 12px 0;
  font-size: 14px;
  word-wrap: break-all;
}

.footer {
  padding: 12px 10px;
  text-align: center;
  display: flex;
  align-items: center;
}

.qrcode {
  width: 100px; 
  padding: 10px;
}

.info {
  padding: 0 12px;
  color: #999;
}

</style>
