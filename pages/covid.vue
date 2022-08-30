<template>
  <section>
    <Header />
    <div id="data-covid" class="container mt-4">
      <div v-if="error" class="text-red-700">
        <ErrorAlert :message="error.message"></ErrorAlert>
      </div>
      <div class="text-center">
        <h1>Data Covid-19 Seluruh Provinsi di Indonesia</h1>
      </div>
      <p>
        Sumber :
        <a href="https://github.com/Reynadi531/api-covid19-indonesia-v2"
          >https://github.com/Reynadi531/api-covid19-indonesia-v2</a
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
      <div class="col-lg-12">
        <table class="table table-striped mt-4">
          <thead>
            <tr>
              <th class="col-lg-4">Provinsi</th>
              <th class="col-lg-2">Kasus</th>
              <th class="col-lg-2">Dirawat</th>
              <th class="col-lg-2">Sembuh</th>
              <th class="col-lg-2">Meninggal</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="list in filteredList" v-bind:key="list.id">
              <td class="col-lg-4">{{ list.provinsi }}</td>
              <td class="col-lg-2">{{ list.kasus | numFormat }}</td>
              <td class="col-lg-2">{{ list.dirawat | numFormat }}</td>
              <td class="col-lg-2">{{ list.sembuh | numFormat }}</td>
              <td class="col-lg-2">{{ list.meninggal | numFormat }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </section>
</template>
<script>
import Vue from 'vue'
import numeral from 'numeral'
import numFormat from 'vue-filter-number-format'
import axios from 'axios'
import Header from '../components/Header.vue'

Vue.filter('numFormat', numFormat(numeral))

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
      .get('https://apicovid19indonesia-v2.vercel.app/api/indonesia/provinsi')
      .then((response) => {
        this.list = response.data
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
        if (item.provinsi.toLowerCase().indexOf(searchQuery) !== -1) {
          return item
        }
      })

      return lists
    },
  },
  components: { Header },
}
</script>
