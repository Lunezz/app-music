<template lang="pug">
#app
	img(src='./assets/logo.png')
	h1 App music
	select(v-model="selectedCountry")
		option(v-for="country in countries" :value="country.value") {{country.name}}
	spinner(v-show="loading")
	ul
		artist(v-for="artist in artists" v-bind:artist="artist" :key="artist.mbid")
  
 
</template>

<script>
import Artist from './components/Artist.vue'
import getArtists from './api'
import spinner from './components/spinner.vue'

export default {
  name: 'app',
  data () {
    return {
			artists:[],
			countries: [
				{name: 'Argentina', value: 'Argentina'},
				{name: 'México', value: 'Mexico'},
				{name: 'España', value: 'Spain'},
			
			],
			selectedCountry: 'Mexico',
			loading: true,
    }
	},
	components:{
		Artist,
		spinner
	},

	methods:{
		refreshArtist(){
			const self = this
			this.loading = true
			this.artists = []
			getArtists(this.selectedCountry)
				.then(function (artists){
					self.artists = artists
					self.loading = false
				})
			}
	},
	mounted(){
		this.refreshArtist();
	},
	watch: {
		selectedCountry: function(){
			this.refreshArtist();
		}
	}
}
</script>

<style lang="stylus">
#app
  font-family 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color #2c3e50
  margin-top 60px

h1, h2
  font-weight normal

ul
  list-style-type none
  padding 0

li
  display inline-block
  margin 0 10px

a
  color #42b983
</style>
