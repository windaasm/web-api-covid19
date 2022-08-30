<template>
  <section>
    <Header />
    <div id="index" class="container mt-4">
      <div v-if="error" class="text-red-700">
        <ErrorAlert :message="error.message"></ErrorAlert>
      </div>
      <div class="text-center"><h1>Faskes Vaksin Provinsi Aceh</h1></div>
      <p>
        Sumber :
        <a href="https://covid19.go.id/dokumentasi-api-faskes-vaksinasi"
          >https://covid19.go.id/dokumentasi-api-faskes-vaksinasi</a
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
              <th class="col-lg-2">Jenis Faskes</th>
              <th class="col-lg-2">Nama Puskesmas</th>
              <th class="col-lg-2">Kota</th>
              <th class="col-lg-1">Provinsi</th>
              <th class="col-lg-1">Status</th>
              <th class="col-lg-3">Alamat</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="list in filteredList" v-bind:key="list.id">
              <td class="col-lg-2">{{ list.jenis_faskes }}</td>
              <td class="col-lg-2">{{ list.nama }}</td>
              <td class="col-lg-2">{{ list.kota }}</td>
              <td class="col-lg-1">{{ list.provinsi }}</td>
              <td class="col-lg-2">{{ list.status }}</td>
              <td class="col-lg-3">{{ list.alamat }}<br />{{ list.telp }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </section>
</template>
<script>
import axios from 'axios'

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
      .get('https://kipi.covid19.go.id/api/get-faskes-vaksinasi?skip=0&city')
      .then((response) => {
        this.list = response.data.data
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
        if (item.nama.toLowerCase().indexOf(searchQuery) !== -1) {
          return item
        }
        if (item.jenis_faskes.toLowerCase().indexOf(searchQuery) !== -1) {
          return item
        }
        if (item.kota.toLowerCase().indexOf(searchQuery) !== -1) {
          return item
        }
      })

      return lists
    },
  },
}
</script>
