<template>
  <div class="form-control">
    <label for="type">Тип блока</label>
    <select id="type" @input="change">
      <option v-for="option in options" :value="option.value" :selected="option.value === selected">{{option.text}}</option>
    </select>
  </div>
</template>

<script>
export default {
  emits: {
    'update:select'(str) {
      if (str) {
        return true
      }
      console.warn('No data in select emit')
      return false
    }
  },
  props: {
    selected: {
      type: String,
      required: false,
      default: ''
    },
    options: {
      type: Array,
      required: true
    }
  },
  methods: {
    change(e) {
      this.$emit('update:select', e.target.value)
    }
  }
}
</script>

<style>

</style>
