<template>
  <div class="bg-zinc-800 min-h-screen text-white home">
    <!-- Header -->
    <Header />

    <!-- search -->
    <div class="container search">
      <b-form-input v-model.lazy="searchDummyInput" type="text" placeholder="Search Movie Name" @keyup.enter="$fetch"
        @keyup.delete="clearSearch"></b-form-input>
      <b-button v-show="searchInput !== ''" class="ml-2 bg-primary" variant="" size="sm" @click="clearSearch">Clear
        Search</b-button>
    </div>

    <!-- Loading -->
    <Loading v-if="$fetchState.pending" />

    <!-- movie -->
    <div v-else class="container movies">
      <!-- search movie -->
      <div v-if="searchInput !== ''" id="movie-grid" class="movies-grid">
        <div v-for="(movie, index) in searchedMovies" :key="index" class="movie">
          <div class="movie-img">
            <div v-if="movie.poster_path != null">
              <img :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`" />
            </div>
            <div v-else>
              <img src="@/assests/image/noimage.png" />             
            </div>
            <p class="review">{{ movie.vote_average }}</p>
            <p class="overview">{{ movie.overview }}</p>
          </div>
          <div class="info">
            <p class="title">
              {{ movie.title.slice(0, 25) }}
              <span v-if="movie.title.length > 25">...</span>
            </p>
            <p class="release">
              Released:
              {{
                  new Date(movie.release_date).toLocaleString('en-us', {
                    month: 'long',
                    day: 'numeric',
                    year: 'numeric',
                  })
              }}
            </p>
            <b-button variant="outline-primary">
              <NuxtLink class="" :to="{ name: 'movies-movieid', params: { movieid: movie.id } }">
                Get More Info
              </NuxtLink>
            </b-button>
            <b-button variant="outline-primary">
              <NuxtLink class="" :to="{ name: 'comments-commentid', params: { commentid: movie.id } }">
                See Article
              </NuxtLink>
            </b-button>
          </div>
        </div>
      </div>
      <!-- now streaming -->
      <div v-else id="movie-grid" class="movies-grid">
        <div v-for="(movie, index) in movies" :key="index" class="movie">
          <div class="movie-img">
            <img :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`" />
            <p class="review">{{ movie.vote_average }}</p>
            <p class="overview">{{ movie.overview }}</p>
          </div>
          <div class="info">
            <p class="title">
              {{ movie.title.slice(0, 25) }}
              <span v-if="movie.title.length > 25">...</span>
            </p>
            <p class="release">
              Released:
              {{
                  new Date(movie.release_date).toLocaleString('en-us', {
                    month: 'long',
                    day: 'numeric',
                    year: 'numeric',
                  })
              }}
            </p>
            <b-button variant="outline-primary">
              <NuxtLink class="" :to="{ name: 'movies-movieid', params: { movieid: movie.id } }">
                Get More Info
              </NuxtLink>
            </b-button>
            <b-button variant="outline-primary">
              <NuxtLink class=""
                :to="{ name: 'comments-commentid', params: { commentid: movie.id, moviename: movie.title } }">
                See Article
              </NuxtLink>
            </b-button>
          </div>
        </div>
      </div>
    </div>

    <!-- page botton -->
    <div class="justify-center flex gap-2 pb-8">     
    </div>

  </div>
</template>

<script>
import axios from 'axios'
import Header from '@/components/Hea-der.vue'
import Loading from '@/components/Loading.vue'

export default {
  name: 'IndexPage',
  components: {
    Header,
    Loading
  },  

  data() {
    return {
      movies: [],
      searchInput: '',
      searchDummyInput: '',
      searchedMovies: [],
      // url: 'https://backend-final.azurewebsites.net'
      url: 'http://localhost:3000'
    }
  },
  async fetch() {
    if (this.searchDummyInput === '') {
      await this.getMovies()

    } else {

      await this.searchMovies()

    }
  },
  fetchDelay: 2000,

  methods: {
    async getMovies() {      
      try {
        // const data = axios.get(`https://api.themoviedb.org/3/movie/now_playing?api_key=855c67ea42890d4442543dfe2e92447f&language=en-US&page=1`)
        const data = axios.get(this.url + "/movies/1")

        const result = await data

        console.log('movie:')
        console.log(result.data.data.results)
        console.log('movies:')

        result.data.data.results.forEach((movie) => {
          this.movies.push(movie)
        })
        console.log(this.movies)
      }
      catch (error) { console.log(`get movie failed: ${error}`) }
    },

    async searchMovies() {
      console.log('Searchmovies:')
      this.searchInput = this.searchDummyInput
      // const data = axios.get(`https://api.themoviedb.org/3/search/movie?api_key=855c67ea42890d4442543dfe2e92447f&language=en-US&page=1&query=${this.searchInput}`)
      const data = axios.get(`${this.url}/moviessearch/${this.searchInput}`)

      const result = await data
      console.log('Searchmovies2:')
      result.data.data.results.forEach((searchMovie) => {
        this.searchedMovies.push(searchMovie)
        console.log(this.searchedMovies)
      })
    },

    clearSearch() {
      this.searchDummyInput = ''
      this.searchInput = ''
      this.searchedMovies = []
    }
  },

}
</script>

<style lang="scss" scoped>
// app transitions
.page-enter-active,
.page-leave-active {
  transition: opacity 0.5s;
}

.page-enter,
.page-leave-to {
  opacity: 0;
}

.home {
  .loading {
    padding-top: 120px;
    align-items: flex-start;
  }


  .search {
    display: flex;
    padding: 32px 16px;

    input {
      max-width: 350px;
      width: 100%;
      padding: 12px 6px;
      font-size: 14px;
      border: none;

      &:focus {
        outline: none;
      }
    }

    // .button {
    //   border-top-left-radius: 0;
    //   border-top-right-radius: 0;
    //   border-bottom-left-radius: 0;
    //   border-bottom-right-radius: 0;
    // }
  }
}

.movies {
  padding: 32px 16px;

  .movies-grid {
    display: grid;
    column-gap: 32px;
    row-gap: 64px;
    grid-template-columns: 1fr;

    @media (min-width: 500px) {
      grid-template-columns: repeat(2, 1fr);
    }

    @media (min-width: 750px) {
      grid-template-columns: repeat(2, 1fr);
    }

    @media (min-width: 1100px) {
      grid-template-columns: repeat(4, 1fr);
    }

    .movie {
      position: relative;
      display: flex;
      flex-direction: column;

      .movie-img {
        position: relative;
        overflow: hidden;

        &:hover {
          .overview {
            transform: translateY(0);
          }
        }

        img {
          display: block;
          width: 100%;
          height: 100%;
        }

        .review {
          position: absolute;
          top: 0;
          left: 0;
          display: flex;
          justify-content: center;
          align-items: center;
          width: 40px;
          height: 40px;
          background-color: #007bff;
          color: #fff;
          border-radius: 0 0 16px 0;
          box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1),
            0 2px 4px -1px rgba(0, 0, 0, 0.06);
        }

        .overview {
          line-height: 1.5;
          position: absolute;
          bottom: 0;
          background-color: #007bff;
          padding: 12px;
          color: #fff;
          transform: translateY(100%);
          transition: 0.3s ease-in-out all;
        }
      }

      .info {
        margin-top: auto;

        .title {
          margin-top: 8px;
          color: #fff;
          font-size: 20px;
        }

        .release {
          margin-top: 8px;
          color: #c9c9c9;
        }

        .button {
          margin-top: 8px;
        }
      }
    }
  }
}
</style>
