<template>
  <section>
    <Header />
    <div id="news" class="container mt-4">
      <div v-if="error" class="text-red-700">
        <ErrorAlert :message="error.message"></ErrorAlert>
      </div>
      <div class="text-center">
        <h1>Berita Terkini dan Terpercaya Indonesia</h1>
      </div>
      <p>
        Sumber :
        <a href="https://api-berita-indonesia.vercel.app/antara/terbaru/"
          >https://api-berita-indonesia.vercel.app/antara/terbaru/</a
        >
      </p>
      <div class="col-lg-3">
        <input
          class="form-control"
          type="text"
          placeholder="Search"
          v-model="searchQuery"
        />
      </div>
    </div>
    <div class="container p-4">
      <div class="row col-lg-12">
        <div
          class="card zoom me-4 py-3 mb-4"
          v-for="list in filteredList"
          v-bind:key="list.id"
          style="width: 18rem"
        >
          <img
            :src="list.thumbnail"
            class="card-img-top"
            :alt="list.thumbnail"
          />
          <div class="card-body">
            <div class="card-header bg-white col-lg-12 mb-3">
              {{ formatDate(list.pubDate) }}
            </div>
            <h5 class="card-title" style="color: blue">
              {{ list.title }}
            </h5>
            <p class="card-text">
              {{ list.description }}
            </p>
          </div>
          <div class="card-footer col-lg-12 bg-white">
            <a :href="list.link" class="btn btn-outline-primary mt-3"
              >Selengkapnya</a
            >
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
<style>
.zoom {
  transition: transform 0.2s; /* Animation */
}

.zoom:hover {
  transform: scale(
    1.1
  ); /* (150% zoom - Note: if the zoom is too large, it will go outside of the viewport) */
}
</style>
<script>
import axios from 'axios'
import Header from '~/components/Header.vue'

export default {
  data() {
    return {
      error: '',
      searchQuery: '',
      list: null,
    }
  },
  mounted() {
    axios
      .get('https://api-berita-indonesia.vercel.app/antara/terbaru/')
      .then((response) => {
        this.list = response.data.data.posts
      })
      .catch((error) => console.log(error))
  },
  computed: {
    filteredList: function () {
      var lists = this.list
      var searchQuery = this.searchQuery

      if (!searchQuery) {
        return lists
      }

      searchQuery = searchQuery.trim().toLowerCase()
      lists = lists.filter(function (item) {
        if (item.title.toLowerCase().indexOf(searchQuery) !== -1) {
          return item
        }
        if (item.pubDate.toLowerCase().indexOf(searchQuery) !== -1) {
          return item
        }
      })

      return lists
    },
  },
  components: { Header },
  methods: {
    formatDate(date) {
      const options = { year: 'numeric', month: 'long', day: 'numeric' }
      return new Date(date).toLocaleDateString('en', options)
    },
  },
}
</script>
