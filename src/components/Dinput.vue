<template>
  <div class="d-input">
    <input class="d-input_inner" :type="type" :value="currentValue" ref="input" @input="handleInput" :placeholder="placeholder">
    <div class="d-input_append" v-if="$slots.append">
      <slot name="append"></slot>
    </div>
  </div>
</template>
<script>
export default {
  name: 'DInput',
  componentName: 'DInput',
  data() {
    return {
      currentValue: this.value
    };
  },
  props: {
    value: [String, Number],
    placeholder: String,
    type: {
      type: String,
      default: 'text'
    }
  },
  watch: {
    'value' (val, oldValue) {
      this.setCurrentValue(val);
    }
  },
  methods: {
    handleInput(event) {
      const value = event.target.value;
      this.$emit('input', value);
      this.setCurrentValue(value);
      this.$emit('change', value);
    },
    setCurrentValue(value) {
      if (value === this.currentValue) return;
      this.currentValue = value;
    }
  }
};

</script>
<style>
@charset "UTF-8";
.d-input {
  width: 100%;
  display: inline-table;
}


.d-input_inner {
  display: table-cell;
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  background-color: #fff;
  border-radius: 4px;
  border: 1px solid #bfcbd9;
  color: #1f2d3d;
  font-size: inherit;
  height: 36px;
  line-height: 1;
  outline: 0;
  padding: 3px 10px;
  transition: border-color .2s cubic-bezier(.645, .045, .355, 1)
}

.d-input_inner {
  width: 100%;
  vertical-align: middle;
  display: table-cell;
  box-sizing: border-box;
  background-image: none;
  border-top-right-radius: 0;
  border-bottom-right-radius: 0;
  border-right: 0
}

.d-input_inner::-webkit-input-placeholder {
  color: #97a8be
}

.d-input_inner::-moz-placeholder {
  color: #97a8be
}

.d-input_inner:-ms-input-placeholder {
  color: #97a8be
}

.d-input_inner::placeholder {
  color: #97a8be
}

.d-input_append {
  background-color: #fbfdff;
  color: #97a8be;
  vertical-align: middle;
  display: table-cell;
  position: relative;
  border: 1px solid #bfcbd9;
  border-radius: 4px;
  padding: 0 10px;
  width: 1px;
  white-space: nowrap
}

.d-input_append {
  border-top-left-radius: 0;
  border-bottom-left-radius: 0
}

.d-input_append {
  display: table-cell; 
}

</style>
