<template>
  <div id="container" class='search'>
   <h1>Search</h1>
   <input type='text' v-model='query' @change='getResult(query)'>
   <div id="movie-list">
    <div v-for='result in results' :key='result.id' :id="result.id" class="movie">
      <img class="poster" v-bind:src="`http://image.tmdb.org/t/p/w500${result.poster_path}`" :alt="`Poster de la película ${result.title}`">
      <div class="movie-footer">
        <div class="title">{{result.title}}</div>
      </div>
     </div>
    </div>
    <div class="show-more" @click="showMore(query)">Mostrar más</div>
  </div>
 </template>
<script>
import axios from 'axios'
export default {
 name: 'search',
 data () {
  return {
   query: '',
   results: [],
   page: 1,
  }
 },
 methods: {
  getResult(query) {
   axios.get(`https://api.themoviedb.org/3/search/movie?api_key=039e4f7f61c4c831908c02f8c3e9aba0&language=es-ES&query=${query}&page=${this.page}&include_adult=false`)
   .then(response => {
      this.results = [...this.results, ...response.data.results]
    })
   },
   showMore(query) {
    this.page++;
    this.getResult(query);
   }
 }
}
</script>

<style>
#container {
  max-width: 1280px;
  width: 95%;
  margin: 0 auto;
  margin-top: 5px;
  margin-bottom: 25px;
}

#movie-list {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
  grid-gap: 30px;
  margin-top: 25px;
}

.movie {
  box-shadow: 0 0 22px 1px rgb(0 0 0 / 10%);
  background-color: #232323;
  border-radius: 10px;
}

.movie:hover {
  box-shadow: 0 0 22px 3px rgb(0 0 0 / 25%);
  transition: 0.25s;
  transform: scale(1.1);
  cursor: pointer;
}

.poster {
  width: 100%;
  display: block;
  margin: auto;
  border-radius: 10px 10px 0 0;
  aspect-ratio: 2/3;
}

.movie-footer {
  margin-top: 10px;
  color: white;
  margin-bottom: 2%;
  padding-left: 2%;
  padding-right: 2%;
  padding-bottom: 2%;
}

.title {
  text-align: center;
  font-size: large;
  margin-top: 10%;
}

@media (max-width: 899px) {
  #movie-list {
    grid-template-columns: 1fr 1fr 1fr;
  }
}

|@media (max-width: 599px) {
  #movie-list {
    grid-template-columns: 1fr 1fr;
  }
}

</style>
