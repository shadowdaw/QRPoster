<template>
  <div @click="fileUpload">
    <slot></slot>
    <input type="file" class="hidden" ref="uploader" @change="afterLoad">
  </div>
</template>
<script>
export default {
  name: 'FileUpload',
  methods: {
    fileUpload: function() {
      this.$refs.uploader.click();
    },
    afterLoad: function(e) {
      var vm = this;
      let file = e.target.files[0];
      let fr = new FileReader();
      fr.onload = function(e) { vm.$emit('uploaded', e.target.result); };
      fr.readAsDataURL(file);
    }
  }
}

</script>
