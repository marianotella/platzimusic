<template>
  <div id="app">
    <img src="./assets/logo.png">
    <h1>{{ title }}</h1>
    <select v-model="selectedCountry">
      <option v-for="country in countries" :value="country.value">{{ country.name }}</option>
    </select>
    <spinner v-show="loading"></spinner>
    <ul>
     <artist v-for="artist in artists" v-bind:artist="artist" v-bind:key="artist.mbid"></artist>
    </ul>
  </div>
</template>

<script>
  import Artist from './components/artist.vue'
  import Spinner from './components/spinner.vue'
  import getArtists from './api/'
  import config from './api/config'

  export default {
    name: 'app',
    data () {
      return {
        artists: [],
        countries: [
          { name: 'Argentina', value: 'argentina'},
          { name: 'España', value: 'spain'},
          { name: 'Colombia', value: 'colombia'},
          { name: 'Japon', value: 'japan'},
        ],
        selectedCountry: 'argentina',
        title: config.appname,
        loading: true
      }
    },
    components: {
      Artist,
      Spinner
    },
    methods: {
      refreshArtist(){
        const self = this
        this.loading = true
        this.artists = []
          getArtists(this.selectedCountry)
          .then(function(artists){
            self.artists = artists
            self.loading = false
          })
      }
    },
    mounted(){
      this.refreshArtist()
    },
    watch:{
      selectedCountry: function(){
        this.refreshArtist()
      }
    }
  }
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>