<template>
  <div class="card card-w70">
    <h3 v-if="this.resume.length === 0">Добавьте первый блок, чтобы увидеть результат</h3>

    <component v-else v-for="item in this.resume" :key="item.title" :is="'app-' + item.title" v-bind="{text: item.text}">
    </component>
  </div>
</template>

<script>
import AppTitle from "./AppTitle"
import AppSubTitle from "./AppSubTitle"
import AppAvatar from "./AppAvatar"
import AppText from "./AppText"

export default {
  props: {
    resume: {
      type: Object,
      required: true
    }
  },

  components: {
    'app-title': AppTitle,
    'app-subtitle': AppSubTitle,
    'app-avatar': AppAvatar,
    'app-text': AppText
  }
}
</script>
