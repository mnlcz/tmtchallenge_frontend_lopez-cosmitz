<template>
  <div class='container container-form'>
    <form @submit.prevent='postear'>
      <h2 class='mb-3'>Nuevo post</h2>
      <div class='input'>
        <label for='title'>Titulo</label>
        <input
          v-model='titulo'
          class='form-control'
          type='text'
          id='titulo'
          placeholder='Acá va el título.'
        />
      </div>
      <div class='input'>
        <label for='content'>Contenido</label>
        <textarea
          v-model='contenido'
          rows='5'
          class='form-control'
          type='text'
          id='content'
          placeholder='Escribí acá tu posteo.'
        />
      </div>
      <button type='submit' class='mt-4 btn-pers' id='post_button'>
        Postear
      </button>
      <div
        class='alert alert-warning alert-dismissible fade show mt-5'
        role='alert'
        v-if='errorMessage'
        id='alert_1'
      >
        {{ errorMessage }}
        <button
          type='button'
          class='btn-close'
          data-bs-dismiss='alert'
          aria-label='Close'
          @click='dismissAlert'
        ></button>
      </div>
    </form>
  </div>
</template>

<script>
import Cookies from 'js-cookie'
import { post } from '@/services/api'

export default {
  data() {
    return {
      titulo: '',
      contenido: '',
      errorMessage: ''
    }
  },
  methods: {
    postear() {
      if (this.titulo !== '' && this.contenido !== '') {
        this.errorMessage = ''
        const user = Cookies.get('user')
        if (!user) {
          throw new Error('No estás logueado')
        } else {
          const data = {
            title: this.titulo,
            content: this.contenido
          }
          try {
            post(user, data)
            this.$router.push('/')
          } catch (err) {
            this.errorMessage = err.message
            this.showAlert()
          }
        }
      } else {
        this.errorMessage = 'Completa tu posteo'
        this.showAlert()
      }
    },
    showAlert() {
      const alert = document.getElementById('alert_1')
      alert.classList.remove('d-none')
    },
    dismissAlert() {
      const alert = document.getElementById('alert_1')
      alert.classList.add('d-none')
    }
  }
}
</script>
