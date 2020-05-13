<template>
  <div>
    <div class="container">
      <div class="d-flex justify-content-center mt-5 input-group">
        <input
          type="text"
          v-model="searchInput"
          placeholder="type a film name"
          class="form-control"
        />
        <div class="input-group-append">
          <button class="btn btn-primary" @click="searchFilm">search</button>
        </div>
      </div>
      <div class="d-flex justify-content-center mt-5 input-group">
        <button v-if="isNotFirstPage" class="btn btn-primary mr-5" @click="prevPage">prev</button>
        <button v-if="isLastPage" class="btn btn-primary" @click="nextPage">next</button>
      </div>
    </div>
    <div class="container">
      <div class="row mt-5">
        <div v-for="(film, index) in filmsList" :key="index" class="col-md-4 col-lg-3 card-item">
          <img :src="getImage(film.Poster)" alt="image" />
          <span class="mt-2">Year: {{film.Year}}</span>
          <span class="mt-1">{{ film.Title }}</span>
        </div>
      </div>
    </div>

    <div
      v-if="filmsList === undefined"
      class="d-flex justify-content-center mt-5"
    >Nothing to show.Try request</div>
  </div>
</template>

<script>
import axios from "axios";

import key from "../../global/key";

export default {
  name: "Search",
  data: () => ({
    filmsList: [],
    searchInput: "",
    totalRes: 0,
    page: 1,
    RES_PER_PAGE: 10
  }),
  computed: {
    isNotFirstPage() {
      return this.page > 1;
    },
    isLastPage() {
      const totalPages = Math.round(this.totalRes / this.RES_PER_PAGE);

      return this.totalRes ? this.page !== totalPages : false;
    }
  },
  created() {},
  methods: {
    searchFilm() {
      this.page = 1;
      axios
        .get(`http://www.omdbapi.com/?apikey=${key.code}&s=${this.searchInput}`)
        .then(res => {
          this.filmsList = res.data.Search;
          this.totalRes = res.data.totalResults;
        });
    },
    getImage(poster_path) {
      return `${poster_path}`;
    },
    getData() {
      axios
        .get(
          `http://www.omdbapi.com/?apikey=${key.code}&s=${this.searchInput}&page=${this.page}`
        )
        .then(res => {
          this.filmsList = res.data.Search;
        });
    },
    nextPage() {
      this.page = this.page + 1;
      this.getData();
    },
    prevPage() {
      this.page = this.page - 1;
      this.getData();
    }
  }
};
</script>

<style scoped>
.card-item {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-bottom: 30px;
}
.card-item img {
  display: block;
  max-width: 100%;
  height: 300px;
  object-fit: cover;
}
</style>
