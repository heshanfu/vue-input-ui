<template>
  <div
    ref="parent"
    :class="[{
      'is-focused': isFocus,
      'has-value': value,
      'has-error': error,
      'is-disabled': disabled,
      'is-dark': dark
    }, size]"
    class="field"
    @click="focusInput"
  >
    <input
      :id="id"
      ref="VueInputUi"
      v-model="inputValue"
      :placeholder="label"
      :disabled="disabled"
      :style="[borderStyle]"
      :type="type"
      class="field-input"
      :readonly="readonly"
      @focus="onFocus"
      @blur="onBlur"
      @click="$emit('click')"
    >
    <label
      ref="label"
      :for="id"
      :class="error ? 'text-danger' : null"
      :style="[colorStyle]"
      class="field-label"
      @click="focusInput"
    >
      {{ hint || label }}
    </label>
  </div>
</template>

<script>
  export default {
    name: 'VueInputUi',
    props: {
      value: { type: [String, Object], required: false, default: null },
      label: { type: String, default: 'Enter text' },
      hint: { type: String, default: String },
      error: { type: Boolean, default: Boolean },
      color: { type: String, default: 'dodgerblue' },
      disabled: { type: Boolean, default: false },
      dark: { type: Boolean, default: false },
      id: { type: String, default: 'VueInputUi' },
      size: { type: String, default: String },
      type: { type: String, default: 'text' },
      readonly: { type: Boolean, default: false },
    },
    data: function () {
      return {
        isFocus: false
      }
    },
    computed: {
      borderStyle () {
        const cond = (this.isFocus && !this.error)
        return cond
          ? { border: `1px solid ${this.color} !important` }
          : null
      },
      colorStyle () {
        const cond = this.isFocus
        return cond
          ? { color: `${this.color}` }
          : null
      },
      inputValue: {
        get () {
          return this.value
        },
        set (value) {
          this.$emit('input', value)
        }
      }
    },
    methods: {
      focusInput () {
        this.$refs.VueInputUi.focus()
      },
      onFocus: function () {
        this.$emit('focus')
        this.isFocus = true
      },
      onBlur: function () {
        this.$emit('blur')
        this.isFocus = false
      }
    }
  }
</script>

<style lang="scss" scoped>
  *, *::before, *::after {
    box-sizing: border-box;
  }
  .field{
    position: relative;
    &.is-dark {
      .field-label{
        color: rgba(255, 255, 255, 0.70);
      }
      .field-input{
        background-color: #424242;
        border-color: rgba(255, 255, 255, 0.70);
        color: rgba(255, 255, 255, 0.70);
      }
      &.is-disabled {
        .field-label, .field-input {
          color: #000;
        }
      }
    }
    .field-label{
      position: absolute;
      top: 3px;
      cursor: pointer;
      left: 13px;
      -webkit-transform: translateY(25%);
      transform: translateY(25%);
      opacity: 0;
      -webkit-transition: all 0.25s cubic-bezier(0.645, 0.045, 0.355, 1);
      transition: all 0.25s cubic-bezier(0.645, 0.045, 0.355, 1);
      font-size: 11px;
      color: rgba(0, 0, 0, 0.54);
    }
    .field-input{
      cursor: pointer;
      background-color: #FFF;
      -webkit-transition-duration: 0.3s;
      transition-duration: 0.3s;
      position: relative;
      width: 100%;
      height: 42px;
      min-height: 42px;
      padding: 0 12px;
      font-weight: 400;
      -webkit-appearance: none;
      outline: none;
      border: 1px solid rgba(0, 0, 0, 0.2);
      border-radius: 4px;
      font-size: 14px;
      z-index: 0;
    }
    &.has-error {
      .field-input {
        border-color: orangered !important;
      }
    }
    &.has-value {
      .field-label {
        opacity: 1;
        -webkit-transform: translateY(0);
        transform: translateY(0);
        font-size: 11px;
      }
      .field-input {
        padding-top: 14px;
      }
    }
    &.is-focused {
      .field-input {
        border-color: dodgerblue;
      }
      .field-label {
        color: dodgerblue;
      }
    }
    &.is-disabled {
      .field-input {
        border-color: #CCC;
        background: #F2F2F2;
      }
      .field-label, .field-input {
        cursor: default;
      }
    }
    .text-danger {
      color: orangered !important;
    }
    &.is-dark {
      ::-webkit-input-placeholder { /* WebKit, Blink, Edge */
        color: rgba(255, 255, 255, 0.70);
      }
      :-moz-placeholder { /* Mozilla Firefox 4 to 18 */
        color: rgba(255, 255, 255, 0.70);
        opacity:  1;
      }
      ::-moz-placeholder { /* Mozilla Firefox 19+ */
        color: rgba(255, 255, 255, 0.70);
        opacity:  1;
      }
      :-ms-input-placeholder { /* Internet Explorer 10-11 */
        color: rgba(255, 255, 255, 0.70);
      }
      ::-ms-input-placeholder { /* Microsoft Edge */
        color: rgba(255, 255, 255, 0.70);
      }
      ::placeholder { /* Most modern browsers support this now. */
        color: rgba(255, 255, 255, 0.70);
      }
      &.is-disabled {
        ::-webkit-input-placeholder { /* WebKit, Blink, Edge */
          color: #424242;
        }
        :-moz-placeholder { /* Mozilla Firefox 4 to 18 */
          color: #424242;
          opacity:  1;
        }
        ::-moz-placeholder { /* Mozilla Firefox 19+ */
          color: #424242;
          opacity:  1;
        }
        :-ms-input-placeholder { /* Internet Explorer 10-11 */
          color: #424242;
        }
        ::-ms-input-placeholder { /* Microsoft Edge */
          color: #424242;
        }
        ::placeholder { /* Most modern browsers support this now. */
          color: #424242;
        }
      }
    }
    &.sm {
      .field-input {
        height: 36px;
        min-height: 36px;
        font-size: 12px;
      }
      .field-label {
        font-size: 10px;
      }
      &.has-value {
        .field-input {
          padding-top: 12px;
        }
      }
    }
    &.lg {
      .field-input {
        height: 48px;
        min-height: 48px;
        font-size: 16px;
      }
      .field-label {
        font-size: 14px;
      }
      &.has-value {
        .field-input {
          padding-top: 16px;
        }
      }
    }
  }
</style>