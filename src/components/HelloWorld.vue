  <template>  
 <b-container class="container"> 
  <b-row class="search-wrapper" :gutter="10">
    
    <b-col>
      <b-input placeholder="Filter by Name" icon="search" v-model="filter"></b-input>
      <b-button @click="reset">Reset</b-button>
    </b-col>
      
    <b-col class="col-space"> &nbsp; </b-col>
     
    <b-col>
      <b-select v-model="sort" placeholder="Sort by">
        <option
          v-for="(item, index) in options"
          :label="item.label"
          :value="item.field"
          :key="index">
        </option>
      </b-select>
    </b-col>
      
  </b-row> <!-- search wrapper -->
    
  <b-row>
  <table>
    <thead>
      <tr>
        <th v-for="(option, index) in options" :key="index"> {{ option.label }} </th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(movie, index) in getMovies" :key="index">
        <th> {{ movie.name ? movie.name : "Information missing"  }} </th>
        <th> {{ movie.year ? movie.year : "Information missing" }} </th>
        <th> {{ movie.genre  ? movie.genre : "Information missing"}} </th>
        <th> {{ movie.duration  ? movie.duration : "Information missing"}} </th>
        <th> {{ movie.rating  ? movie.rating : "Information missing"}} </th>
        <th> {{ movie.votes  ? movie.votes : "Information missing"}} </th>
      </tr>
    </tbody>
  </table>
  <b-col v-if="getMovies.length === 0">
    <div class="box box__empty"> No Match Found</div>
  </b-col>
    

    

  </b-row> <!-- results -->
    
 </b-container>

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
      movies: []
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
      
      if (this.sort == 'votes') {
        return movies.sort(function(a, b) {
          return b.votes - a.votes
        });
      }
      else if (this.sort == 'year') {
        return movies.sort(function(a, b) {
          return b.year - a.year
        });
      }
      else if (this.sort == 'genre') {
        return movies.sort(function (a, b) {
          if (a.genre < b.genre) return -1;
          else if (a.genre > b.genre) return 1;
          return 0;
        });
      }
      else if (this.sort == 'duration') {
        return movies.sort(function(a, b) {
          return b.duration - a.duration
        });
      }
      else if (this.sort == 'name') {
        return movies.sort(function (a, b) {
          if (a.name < b.name) return -1;
          else if (a.name > b.name) return 1;
          return 0;
    });
      }
      else if (this.sort == 'rating') {
        return movies.sort(function(a, b) {
          return b.rating - a.rating
        });
      } 
      else {
        return movies;
      }
      
    }
  }
}



</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
