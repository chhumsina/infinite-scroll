<template>
  <div id="app">
    <header>
      <div class="bg-secondary">
        <div class="container align-middle">
          <div class="row align-middle">
            <div class="col-6 text-light">
              <h1 class="align-middle">
                Vue JS - infinite scroll
              </h1>
            </div>
            <div class="col-6 text-center">
              <img src="./assets/logo.png" height="60px">
            </div>
          </div>
        </div>
      </div>
    </header>

    <div class="container mt-5">
      <transition-group name="fade" appear tag="ul" class="list-unstyled row">
        <li class="media bg-light mb-3" v-for="user in this.users" :key="user.location.postcode" style="width:100%">
          <img :src="user.picture.large" alt="placeholder" class="mr-3">
          <div class="media-body">
            <div class="mt-4 mb-2">
              <h5>{{ user.name.first }}<b> {{ user.name.last }}</b></h5>
              {{ user.gender }} - {{ user.email }} <br>
              <i>{{ user.location.street }} - {{ user.location.city }} - {{ user.location.postcode }}</i>
            </div>
          </div>
        </li>
      </transition-group>
    </div>

  </div>
</template>

<script>
import Axios from 'axios'

export default {

  name: 'App',
  data () {
    return {
      users: []
    }
  },
  mounted () {
    this.fetchUsers()
    this.scrollFetch()
    setInterval(() => {
      this.users.length < 10 ? this.fetchUsers() : clearInterval()
    }, 1000)
  },
  methods: {
    fetchUsers () {
      Axios.get('https://randomuser.me/api').then(reponse => {
        this.users.push(reponse.data.results[0])
      })
    },
    scrollFetch () {
      window.addEventListener('scroll', () => {
        // total height - scroll top + window height
        let bottom = document.documentElement.offsetHeight - (window.innerHeight + document.documentElement.scrollTop)
        if (bottom === 0) {
          this.fetchUsers()
        }
      })
    }
  },
  destroyed () {
    window.removeEventListener('scroll', this.scrollFetch())
  }
}
</script>

<style>
  .fade-enter-active, .fade-leave-active {
    transition: opacity .5s;
  }

  .fade-enter, .fade-leave {
    opacity: 0;
  }
</style>
