<template>
  <section class="section">
    <b-select v-model="perPage">
      <option value="5">5 per page</option>
      <option value="10">10 per page</option>
      <option value="15">15 per page</option>
      <option value="20">20 per page</option>
    </b-select>
    <b-table
        :data="quranData"
        :paginated="isPaginated"
        :per-page="perPage"
        :current-page.sync="currentPage"
        :pagination-simple="isPaginationSimple"
        :pagination-position="paginationPosition"
        :default-sort-direction="defaultSortDirection"
        :sort-icon="sortIcon"
        :sort-icon-size="sortIconSize"
        default-sort="user.first_name"
        aria-next-label="Next page"
        aria-previous-label="Previous page"
        aria-page-label="Page"
        aria-current-label="Current page"
        selected.sync="data[1]"
        :loading="isLoading"
        focusable
        :mobile-cards="isMobileCards">

        <template slot-scope="props">
            <b-table-column field="number_of_surah" label="No." width="40" sortable numeric>
                {{ props.row.number_of_surah }}
            </b-table-column>

            <b-table-column field="name" label="Surah Name" sortable>
                {{ props.row.name }}
            </b-table-column>

            <b-table-column field="number_of_ayah" label="Number of Ayah">
                {{ props.row.number_of_ayah }}
            </b-table-column>

            

              <!-- name: 'Al-Ahzab',
name_translations: {
  ar: 'الأحزاب',
  en: 'The Clans',
  id: 'Golongan Yang Bersekutu'
},
number_of_ayah: 73,
number_of_surah: 33,
place: 'Medina',
recitation: 'https://download.quranicaudio.com/quran/mishaari_raashid_al_3afaasee/033.mp3',
type: 'Madaniyah' -->
        </template>
    </b-table>
  </section>
</template>

<script>
import Card from '~/components/Card'
import axios from 'axios'

export default {
  name: 'HomePage',
  data() {
    return {
      // data: [],
      isPaginated: true,
      isPaginationSimple: false,
      paginationPosition: 'bottom',
      defaultSortDirection: 'asc',
      sortIcon: 'arrow-up',
      sortIconSize: 'is-small',
      currentPage: 1,
      perPage: 10,
      isLoading: true,
      isMobileCards: true
    }
  },
  async asyncData({ error }) {
    try {
      const { data } = await axios.get("http://localhost:3000/api/quran.json")
      return { quranData: data }
    } catch (e) {
      error({ statusCode: 404, message: "Quran data not loaded properly" })
    }
  },
  mounted() {
    this.isLoading = false
  },
  components: {
    Card
  },
  head() {
    return {
      title: 'Read Quran'
    }
  }
}
</script>
