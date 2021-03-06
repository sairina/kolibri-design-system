<template>

  <div class="mh">
    <UiTextbox
      ref="textbox"
      v-model="currentText"
      class="textbox"
      :label="label"
      :disabled="disabled"
      :invalid="showInvalidMessage"
      :error="invalidText"
      :autofocus="autofocus"
      :maxlength="maxlength"
      :autocomplete="autocomplete"
      :autocapitalize="autocapitalize"
      :type="type"
      :min="min"
      :max="max"
      :enforceMaxlength="true"
      :floatingLabel="floatingLabel"
      :multiLine="textArea"
      :rows="3"
      @input="updateText"
      @keydown="emitKeydown"
      @focus="$emit('focus')"
      @blur="$emit('blur')"
    />
  </div>

</template>


<script>

  import UiTextbox from '../keen/UiTextbox';

  /**
   * Handles user input.
   */
  export default {
    name: 'KTextbox',
    components: { UiTextbox },
    inheritAttrs: true,
    props: {
      /**
       * v-model
       */
      value: {
        type: [String, Number],
        default: null,
      },
      /**
       * Label
       */
      label: {
        type: String,
        required: true,
      },
      /**
       * Whether or not disabled
       */
      disabled: {
        type: Boolean,
        default: false,
      },
      /**
       * Whether or not input is invalid
       */
      invalid: {
        type: Boolean,
        default: false,
      },
      /**
       * Text displayed when the user is notified of invalid input
       */
      invalidText: {
        type: String,
        default: null,
      },
      /**
       * Show the `invalidText` even if the user has not focused or change the input.
       * `invalid` must also be `true` for this to take effect.
       */
      showInvalidText: {
        type: Boolean,
        default: false,
      },
      /**
       * Whether or not to autofocus
       */
      autofocus: {
        type: Boolean,
        default: false,
      },
      /**
       * Max allowed length of input
       */
      maxlength: {
        type: Number,
        default: null,
      },
      /**
       * HTML5 autocomplete attribute (`off`, `on`, `name`, `username`, `current-password`, etc)
       */
      autocomplete: {
        type: String,
        default: null,
      },
      /**
       * HTML5 autocapitalize attribute. Used for touch-input enabled UI (`off`, `on`, `words`, etc)
       */
      autocapitalize: {
        type: String,
        default: null,
      },
      /**
       * HTML5 type of input (text, password, number, etc.)
       */
      type: {
        type: String,
        default: 'text',
      },
      /**
       * Minimum value, used when type is 'number'
       */
      min: {
        type: Number,
        default: null,
      },
      /**
       * Maximum value, used when type is 'number'
       */
      max: {
        type: Number,
        default: null,
      },
      /**
       * Display as text area.
       */
      textArea: {
        type: Boolean,
        default: false,
      },
      /**
       * @private
       * Whether or not to display as a floating label
       */
      floatingLabel: {
        type: Boolean,
        default: true,
      },
    },
    data() {
      return {
        currentText: this.value,
        changedOrFocused: false,
      };
    },
    computed: {
      showInvalidMessage() {
        return this.invalid && (this.changedOrFocused || this.showInvalidText);
      },
    },
    watch: {
      value(val) {
        this.currentText = val;
        this.changedOrFocused = true;
      },
    },
    methods: {
      updateText() {
        // v-model is just a :value + @input
        /**
         * Emits input event with new value
         */
        this.$emit('input', this.currentText);
      },
      /**
       * @public
       */
      reset() {
        this.$refs.textbox.reset();
      },
      emitKeydown(e) {
        /**
         * Emits keydown event
         */
        this.$emit('keydown', e);
      },
      /**
       * @public
       * Focuses on the textbox
       */
      focus() {
        this.changedOrFocused = true;
        this.$refs.textbox.$el.querySelector('input').focus();
      },
    },
  };

</script>


<style lang="scss" scoped>

  .textbox {
    max-width: 400px;
  }

  .mh {
    min-height: 72px;
  }

</style>
