<template>
  <div id="app" class="fixapp">
    <transition name="slide">
      <div class="form" v-if="editing" key="edit">
        <div class="qrposter" ref="qrposter">
          <uploader @uploaded="modifyPoster">
            <img :src="imgsrc" alt="" class="poster">
          </uploader>
          <div class="content">
            <h1 class="title" contenteditable="true" v-text="title"></h1>
            <p class="summary" contenteditable="true" v-text="summary"></p>
          </div>
          <div class="footer">
            <uploader @uploaded="modifyQrcode">
              <img :src="qrcodeSrc" alt="" class="qrcode">
            </uploader>
            <div class="info">
              <p>长按二维码查看原文</p>
              <p>生成工具: {{site}}</p>
            </div>
          </div>
        </div>
        <div class="form-item">
          <dinput type="text" placeholder="输入二维码地址或点击二维码图片上传" class="daw-input" v-model="qrurl">
            <span slot="append" @click="initQrCodeSrc">生成二维码</span>
          </dinput>
        </div>
        <div class="form-item">
          <div class="btn" @click="createPoster">生成海报</div>
        </div>
        <p class="tips"><a class="bloglink" href="https://blog.insomnia-er.com/">查看作者个人主页</a></p>
      </div>
      <div class="form" v-else key="save">
        <img :src="qrposter" class="poster">
        <div class="form-item">
          <a class="btn" :href="qrposter" download="qrposter.jpeg">保存至本地</a>
        </div>
        <div class="form-item">
          <div class="btn" @click="reEdit">重新编辑</div>
        </div>
        <p class="tips">若保存失败，请尝试长按图片保存</p>
        <p class="tips"><a class="bloglink" href="https://blog.insomnia-er.com/">查看作者个人主页</a></p>
      </div>
    </transition>
  </div>
</template>
<script>
import Uploader from '@/components/FileUp2Base64';
import Dinput from '@/components/Dinput';
import Qrious from 'qrious';
import html2canvas from 'html2canvas';
export default {
  name: 'App',
  data: function() {
    return {
      qrurl: '',
      title: '点击编辑标题',
      summary: '点击编辑摘要',
      imgsrc: '/static/poster.jpg',
      qrposter: '',
      editing: true,
      qrcodeSrc: '',
      site: window.location.origin
    }
  },
  computed: {},
  created: function() {
    this.initQrCodeSrc();
  },
  methods: {
    initQrCodeSrc: function() {
      this.qrcodeSrc = this.createQrcodeSrc(this.qrurl)
    },
    createQrcodeSrc: function(url) {
      var qr = new Qrious({
        value: url
      });
      return qr.toDataURL();
    },
    modifyPoster: function(imgData) {
      this.imgsrc = imgData
    },
    modifyQrcode: function(imgData) {
      console.log(imgData)
      this.qrcodeSrc = imgData
    },
    createPoster: function() {
      var vm = this;
      html2canvas(this.$refs.qrposter).then(function(canvas) {
        vm.qrposter = canvas.toDataURL('image/jpeg');
        vm.editing = false;
      });
    },
    reEdit: function() {
      this.editing = true;
    }
  },
  components: { Uploader, Dinput }
}

</script>
<style>
#app {
  max-width: 450px;
  margin: 0 auto;
  font-family: Helvetica Neue, Helvetica, PingFang SC, Hiragino Sans GB, Microsoft YaHei, SimSun, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.bloglink {
  margin-top: 12px;
  color: #999;
}

.tips {
  margin-top: 12px;
  color: #409EFF;
}

.hidden {
  display: none;
}

.form {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  padding-bottom: 20px;
}

.qrposter {
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.05);
  margin-bottom: 20px;
}

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
  text-align: left;
  display: flex;
  align-items: center;
}

.qrcode {
  width: 100px;
  height: 100px;
}

.info {
  padding: 0 12px;
  color: #999;
}

.form-item {
  padding: 8px 12px;
}

.btn {
  display: block;
  background-color: #6de4a3;
  color: #FFF;
  height: 30px;
  line-height: 30px;
  font-size: 18px;
  border-radius: 6px;
}

.daw-input {
  width: 100%;
  height: 30px;
  line-height: 30px;
  border: none;
  outline: none;
  box-shadow: 0 0px 3px 4px rgba(0, 0, 0, 0.05);
  -webkit-tap-highlight-color: transparent;
}

.daw-input:focus {
  box-shadow: 0 0px 3px 4px rgba(0, 0, 0, 0.15);
}

.slide-enter-active {
  transition: all 1s ease;
}

.slide-leave-active {
  transition: all 1s ease;
}

.slide-enter {
  opacity: 0;
}

.slide-leave-to {
  opacity: 0;
}

</style>
