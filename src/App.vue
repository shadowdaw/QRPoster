<template>
  <div id="app" class="fixapp">
    <div>
      <transition name="slide">
        <div class="form" v-show="editing" key="edit">
          <div class="qrposter" ref="qrposter">
            <div class="introduction" v-if="firstTime">
              <p>此处是海报模版区域</p>
              <p>按照说明编辑修改</p>
              <p>再点下方“生成海报”</p>
              <p>生成此区域对应图片</p>
              <div class="form-item">
                <div class="btn" @click="iknow">我来试试</div>
              </div>
            </div>
            <normal :qrcodeSrc="qrcodeSrc" v-if="currentTemp=='normal'"/>
            <news :qrcodeSrc="qrcodeSrc" v-else-if="currentTemp=='news'"/>
          </div>
          <div class="form-item">
            <dinput type="text" placeholder="输入二维码地址或点击二维码图片上传" class="daw-input" v-model="qrurl">
              <span slot="append" @click="initQrCodeSrc">生成二维码</span>
            </dinput>
          </div>
          <div class="form-item">
            <div class="btn" @click="createPoster">生成海报</div>
          </div>
          <div class="form-item">
            <div class="btn" @click="switchTemplate">切换模版</div>
          </div>
          <p class="tips"><a class="bloglink" href="https://blog.insomnia-er.com/">查看作者个人主页</a></p>
        </div>
      </transition>
      <transition name="slide">
        <div class="form" v-show="!editing" key="save">
          <div class="box">
            <img :src="qrposter" class="poster card">
          </div>
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
    <transition name="fade">
      <div v-show="showTemplate" class="form templatelist">
        <carousel :watch-items="tempList" :dots="false" :control="false" :loop="true">
          <carousel-item v-for="(temp, index) in tempList" :key="temp">
            <div class="box">
              <img :src="`/static/${temp}.jpeg`" class="poster card">
            </div>
            <div class="form-item">
              <div class="btn" @click="chooseTemplate(temp)">使用此模版</div>
            </div>
          </carousel-item>
        </carousel>
      </div>
    </transition>
  </div>
</template>
<script>
import Uploader from '@/components/FileUp2Base64';
import Dinput from '@/components/Dinput';
import Normal from '@/components/Normal';
import News from '@/components/News';
import Qrious from 'qrious';
import cookies from '@/utils/cookies';
import html2canvas from 'html2canvas';
import { Carousel, CarouselItem } from '@/components/Carousel'
export default {
  name: 'App',
  data: function() {
    return {
      qrurl: '',
      qrposter: '',
      editing: true,
      qrcodeSrc: '',
      site: window.location.origin,
      firstTime: '1' !== cookies.get('used'),
      showTemplate: false,
      tempList: ['normal', 'news'],
      currentTemp:'normal'
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
    createPoster: function() {
      var vm = this;
      html2canvas(this.$refs.qrposter).then(function(canvas) {
        vm.qrposter = canvas.toDataURL('image/jpeg');
        vm.editing = false;
      });
    },
    reEdit: function() {
      this.editing = true;
    },
    iknow: function() {
      this.firstTime = false;
      cookies.set('used', '1')
    },
    switchTemplate: function() {
      this.showTemplate = true;
    },
    chooseTemplate: function(ct) {
      this.currentTemp=ct;
      this.showTemplate = false;
    },
  },
  components: {
    Uploader,
    Dinput,
    Normal,
    News,
    'carousel': Carousel,
    'carousel-item': CarouselItem
  }
}

</script>
<style>
#app {
  max-width: 450px;
  min-height: 100%;
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

.introduction {
  position: absolute;
  background-color: rgba(0, 0, 0, 0.7);
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  color: #FFF;
  font-size: 18px;
}

.qrposter {
  position: relative;
  box-shadow: 0 5px 10px 1px rgba(0, 0, 0, 0.1);
  margin-bottom: 20px;
}

.box {
  padding: 8px;
}

.card {
  box-shadow: 0 0px 8px 5px rgba(0, 0, 0, 0.1);
}
.form-item {
  padding: 8px 12px;
}
.poster {
  width: 100%;
}

.btn {
  display: block;
  background-color: #6de4a3;
  color: #FFF;
  height: 30px;
  line-height: 30px;
  font-size: 18px;
  border-radius: 6px;
  padding: 0 12px;
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
  transform: translateX(100%);
  opacity: 0;
}

.slide-leave-to {
  transform: translateX(-100%);
  opacity: 0;
}

.fade-enter-active {
  transition: all 1s ease;
}

.fade-leave-active {
  transition: all 1s ease;
}

.fade-enter {
  opacity: 0;
}

.fade-leave-to {
  opacity: 0;
}

.v-carousel { 
  width: 100%;
}
.templatelist{
  min-height: 100%;
  background-color: rgba(0,0,0,0.8);
}

</style>
