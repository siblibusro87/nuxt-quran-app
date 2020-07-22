<template>
  <div>
    <h1>{{ surah.name }}</h1>
    <h1>{{ surah.name_translations['en'] }} - {{ surah.name_translations['id'] }}</h1>
    <h1>Surah No. {{ surah.number_of_surah }} of 114 - {{ surah.number_of_ayah }} Ayah</h1>
    <div v-for="verse in surah.verses" :key="verse.number">
      <h1>{{ verse.text }}</h1>
      <h1>{{ verse.translation_en }}</h1>
      <h1>{{ verse.translation_id }}</h1>
      </br>
    </div>
    <br>
    <b-button size="is-large" icon-left="github-circle" @click.prevent="this.isPlaying ? pauseSound(surah.recitations[0].audio_url) : playSound(surah.recitations[0].audio_url)">Listen to {{ surah.name }}</b-button>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'surah',
  data() {
    isPlaying: false
  },
  async asyncData({ params, error }) {
    try {
      const { data } = await axios.get(`http://localhost:3000/api/surah/${params.id}.json`)
      console.log(data)
      return { surah: data }
    } catch (e) {
      error({ statusCode: 404, message: "Surah not found" })
    }
  },
  mounted() {
    console.log(this.surah)
  },
  methods: {
    playSound (sound) {
      if(sound) {
        var audio = new Audio(sound);
        audio.play();

        this.isPlaying == true
      }
    },
    pauseSound (sound) {
      if(sound) {
        var audio = new Audio(sound);
        audio.pause();

        this.isPlaying == false
      }
    }
  },
  head() {
    return {
      title: this.surah.name
    }
  }
}
</script>

<style>

</style>