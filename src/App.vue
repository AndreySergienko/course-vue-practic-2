<template>
  <div class="container column">

    <resume-form @submitHandler="createResume"></resume-form>


    <app-card
      :resume="resume"
    ></app-card>
  </div>
  <div class="container">
    <button class="btn" @click="saveResume">Сохранить резюме</button>
    <button class="btn danger" @click="removeResume">Удалить резюме</button>
    <p>
      <button class="btn primary" @click="loadComments">Загрузить комментарии</button>
    </p>
    <app-loader v-if="isLoading" ></app-loader>
    <app-comments
      v-if="this.comments.length > 0"
      v-for="comment in comments"
      :key="'comment-' + comment.id"
      :id="comment.id"
      :text="comment.body"
      :email="comment.email"
    ></app-comments>
  </div>
</template>

<script>
import ResumeForm from './components/ResumeForm'
import AppLoader from "./components/AppLoader"
import AppComments from "./components/AppComments"
import AppCard from "./components/AppCard"
import axios from "axios"
export default {
  data() {
    return {
      resume: [],
      comments: [],
      isLoading: false
    }
  },
  mounted() {
    this.loadResume()
  },
  methods: {
    createResume(data) {
      this.resume.push({...data})
    },
    async loadComments () {
      try {
        this.isLoading = true
        const {data} = await axios.get('https://jsonplaceholder.typicode.com/comments?_limit=42')
        this.comments = Object.keys(data).map(key => {
          return {
            id: key,
            ...data[key]
          }
        })
      } catch (e) {
        console.log(e)
      } finally {
        this.isLoading = false
      }
    },
    async saveResume() {
      try {
        this.isLoading = true
        await fetch('https://vladilen-minin-databese-default-rtdb.europe-west1.firebasedatabase.app/resume', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
            'Access-Control-Allow-Origin': '*'
          },
          body: JSON.stringify({
            resume: this.resume
          })
        })
        return alert('Резюме успешно сохранено')
      } catch (e) {
        console.log(e)
      } finally {
        this.isLoading = false
      }
    },
    async removeResume() {
      try {
        this.isLoading = true
        await axios.delete('https://vladilen-minin-databese-default-rtdb.europe-west1.firebasedatabase.app/resume')
        return alert('Резюме успешно удалено')
      } catch (e) {
        console.log(e)
      } finally {
         this.isLoading = false
      }
    },
    async loadResume() {
      try {
        this.isLoading = true
        const {data} = await axios.get('https://vladilen-minin-databese-default-rtdb.europe-west1.firebasedatabase.app/resume')
        return this.resume.push({...data})
      } catch (e) {
        console.log(e)
      } finally {
        this.isLoading = false
      }
    }
  },
  components: {
    ResumeForm,
    AppLoader,
    AppComments,
    AppCard
  },
}
</script>

<style>
.avatar {
  display: flex;
  justify-content: center;
}

.avatar img {
  width: 150px;
  height: auto;
  border-radius: 50%;
}
</style>
