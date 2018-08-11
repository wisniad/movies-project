<template>  
  <div>
    <div class="inputs"> 
      <input placeholder="Filter by Name" icon="search" v-model="filter">
      <button @click="reset">Reset</button>
    
      <select v-model="sort">
        <option value="" disabled selected>Sort by</option>
        <option
          v-for="(item, index) in options"
          :label="item.label"
          :value="item.field"
          :key="index">
        </option>
      </select>
    </div>  
    <table>
        <div v-if="getMovies.length === 0 && this.errors.length === 0">Loading movies </div>
        <div v-if="this.errors.length !== 0"> Cannot load data, please check your internet connection </div>

      <thead>
        <tr>
          <th v-for="(option, index) in options" :key="index"> {{ option.label }} </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(movie, index) in getMovies" :key="index">
          <th> {{ movie.name ? movie.name : "Information missing" }} </th>
          <th> {{ movie.year ? movie.year : "Information missing" }} </th>
          <th> {{ movie.genre  ? movie.genre : "Information missing"}} </th>
          <th> {{ movie.duration  ? movie.duration : "Information missing" }} </th>
          <th> {{ movie.rating  ? movie.rating : "Information missing" }} </th>
          <th> {{ movie.votes  ? movie.votes : "Information missing" }} </th>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'HelloWorld',
  data: function() {
    return { 
      filter: '',
      sort: '',
      options: [],
      movies: [],
      errors: []
    }
  },
  mounted () {
      axios.get(`https://api.myjson.com/bins/1tll6`)
        .then((movies) => { 
          movies.data.values.map(movie => {
            this.movies.push(movie)
          })
          movies.data.fields.map( field => {
            this.options.push(field)
          })
        })  
        .catch(e => {
          this.errors.push(e)
        })
  },
  methods: {
    reset () {
      this.filter = ""
    }
  },
  computed: {
    getMovies() {
      let movies = this.movies.filter((movie) => {
          return movie.name.toLowerCase().includes(this.filter.toLowerCase());
        }); 
      let sorting = this.sort
      return movies.sort(function(a, b) {
        switch(sorting) {
          case "year": {
            return b.year - a.year
          }
          case "genre": {
            return (a.genre < b.genre) ? -1 : (a.genre > b.genre)
          }
          case "votes": {
            return b.votes - a.votes
          }
          case "duration": {
            return b.duration - a.duration
          }
          case "name": {
            return (a.name < b.name) ? -1 : (a.name > b.name)
          }
          case "rating": {
            return b.rating - a.rating
          }
          default: {
            return movies;
          }
        }
      })
    }
  }
}



</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

*{ 
  -webkit-box-sizing: border-box; /* Safari/Chrome, other WebKit */
  -moz-box-sizing: border-box;    /* Firefox, other Gecko */
  box-sizing: border-box;         /* Opera/IE 8+ */
}
table{
  padding-top: 3rem;
}
table,thead,tbody,tr,th{
  text-align: center;
  margin: auto;
  padding: 1.5rem;
  width: 50%;
}
tr {
  background: #fff;
  font-weight: 100;
}
thead {
  font-weight: bold;
}

select, input,button{
  text-align: center;
  margin: auto;
  padding: 0.5rem;
  color:#5c5e5d;
}
select{
  margin-left: 2rem;
}
</style>
