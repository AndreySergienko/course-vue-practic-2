<template>
  <div class="form-control">
    <label :for="id">{{label}}</label>
    <textarea :id="id" :rows="rows" :value="textValue" @input="change"></textarea>
  </div>
</template>

<script>
export default {
  emits: {
    'update:textValue'(str) {
      if (str) {
        return true
      }
      console.warn('No data in textValue emit')
      return false
    }
  },
  props: {
    textValue: {
      type: String,
      required: false,
      default: ''
    },
    rows: {
      type: Number,
      required: false,
      default: 5
    },
    label: {
      type: String,
      required: false,
      default: 'Текстовое поле'
    }
  },
  data() {
    return {
      id: 'input-' + Math.random()
    }
  },
  methods: {
    change(e) {
      this.$emit('update:textValue', e.target.value)
    }
  }
}
</script>
