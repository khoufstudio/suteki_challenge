<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
/*eslint no-mixed-spaces-and-tabs: ["error", "smart-tabs"]*/
import HelloWorld from './components/HelloWorld.vue'
import axios from 'axios'

export default {
  name: 'App',
  components: {
    HelloWorld
  },
  data() {
    return {
      baseUrl: process.env.VUE_APP_BASE_URL,
    }
  },
  methods: {
    fetchData() {
      axios.get(this.baseUrl + 'employee.json')
        .then(response => {
          console.log(response.data)
        })
    }
  },
  created() {
    // console.log(this.baseUrl)
    this.fetchData()
  }
}
</script>

<style lang="scss">
  $color: blue;
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: $color;
    margin-top: 60px;
  }
</style>
