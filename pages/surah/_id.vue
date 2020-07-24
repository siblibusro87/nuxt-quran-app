<template>
  <div>
    <div>
      <full-page ref="fullpage" :options="options" id="fullpage">
        <!-- <section v-if="surah.length==0" class="section"></section> -->
        <div v-for="(verse, index) in surah.verses" :key="index" class="section">
          <h1>{{ surah.name }}</h1>
          <br />
          <h1 class="title is-1">{{ verse.text }}</h1>
          <h1>{{ verse.translation_en }}</h1>
          <h1>{{ verse.translation_id }}</h1>
          <h1>{{ surah.name_translations['en'] }} - {{ surah.name_translations['id'] }}</h1>
          <br />
          <h1>{{ index+1 }} of {{ surah.number_of_ayah }} Ayah</h1>
        </div>
      </full-page>
    </div>
    <div>

    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'surah',
  data() {
    return {
      isPlaying: false,
      options: {
        scrollBar: true
      }
    }
  },
  async asyncData({ $axios, params, error }) {
    try {
      const { data } = await $axios.get(`/surah/${params.id}.json`)
      return { surah: data }
    } catch (e) {
      error({ statusCode: 404, message: "Surah not found" })
    }
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