<template>
  <form class="card card-w30" @submit.prevent="$emit('submitHandler', submitForm())">

      <app-select
        v-model:select="selectField"
        :options="options"
        :selected="selectField"
      ></app-select>

      <app-textarea
        label="Значение"
        v-model:textValue="text"
        :rows="3"
        :text="text"
      ></app-textarea>

    <button class="btn primary" type="submit" :disabled="disableBtn">Добавить</button>
  </form>
</template>

<script>
  import AppTextarea from "./AppTextarea"
  import AppSelect from "./AppSelect"
  export default {
    emits: {
      'submitHandler'(obj) {
        if (obj) {
          return true
        }
        console.warn('No data in submitHandler emit')
        return false
      }
    },
    data() {
      return {
        text: '',
        selectField: 'title',
        options: [
          {text: 'Заголовок', value: 'title'},
          {text: 'Подзаголовок', value: 'subtitle'},
          {text: 'Аватар', value: 'avatar'},
          {text: 'Текст', value: 'text'},
        ],
      }
    },
    methods: {
      submitForm() {
          const data = {
            title: this.selectField,
            text: this.text
          }
          this.selectField = 'title'
          this.text = ''
          return data
      }
    },
    computed: {
      disableBtn() {
        return this.text.length < 3 ? true : false
      }
    },
    components: {
      AppTextarea,
      AppSelect
    }
  }
</script>

<style scoped>

</style>
