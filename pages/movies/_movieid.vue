<template>
  <div class="bg-zinc-800 min-h-screen text-white">
    <Loading v-if="$fetchState.pending" />
    <div v-else class="container single-movie">
      <!-- <b-button variant="outline-primary"> -->
      <NuxtLink class="button" :to="{ name: 'index' }">Back</NuxtLink>
      <!-- </b-button> -->
      <div class="movie-info">
        <div class="movie-img">
          <img :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`" />
        </div>

        <div class="movie-content">
          <h1> Titile: {{ movie.title }}</h1>
          <p class="movie-fact tagline">
            <span>Tagline:</span> "{{ movie.tagline }}"
          </p>
          <p class="movie-fact">
            <span>Released:</span>
            {{
                new Date(movie.release_date).toLocaleString('en-us', {
                  month: 'long',
                  day: 'numeric',
                  year: 'numeric',
                })
            }}
          </p>
          <p class="movie-fact">
            <span>Duration:</span> {{ movie.runtime }} minutes
          </p>
          <p class="movie-fact">
            <span>Revenue:</span>
            {{
                movie.revenue.toLocaleString('en-us', {
                  style: 'currency',
                  currency: 'USD',
                })
            }}
          </p>
          <p class="movie-fact">
            <span>Overview:</span>
            {{ movie.overview }}
          </p>        
          <div class="movie-fact">
            <span>Watch Provider:</span>                                  
            <div class="flex gap-8">
              <div>
                <p>TH</p>
                <div class="w-20 mt-2">
                  <div v-if="this.nonTHprovider == ''">
                  <img :src="`https://image.tmdb.org/t/p/w500/${THprovider.TH.flatrate[0].logo_path}`" />
                  </div>
                  <div v-else class="border-2 border-red-600 bg-red-600 pl-1">
                  <p class="text-white font-bold"> {{ nonTHprovider }}</p>
                  </div>
                </div>
              </div>
              <div>
                <p>US</p>
                <div class="w-20 mt-2">
                  <div v-if="this.nonENprovider == ''">
                  <img :src="`https://image.tmdb.org/t/p/w500/${ENprovider.US.flatrate[0].logo_path}`" />
                  </div>
                  <div v-else class="border-2 border-red-600 bg-red-600 pl-1">
                  <p class="text-white font-bold"> {{ nonENprovider }}</p>
                  </div>
                </div>
              </div>
            </div>       
          </div>         
        </div>



      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios"
import Loading from "../../components/Loading.vue"
export default {
  name: "SingleMovie",
  components: { Loading },
  head() {
    return {
      title: this.movie.title
    }
  },
  data() {
    return {
      movie: '',
      THprovider: null,
      ENprovider: null,
      nonTHprovider: '',
      nonENprovider: '',     
      url: 'https://backend-final.azurewebsites.net'
      // url: 'http://localhost:3000'
    }
  },
  async fetch() {
    await this.getSingleMovie();
    await this.getTHProvider();
    await this.getENProvider();
    
  },
  fetchDelay: 2000,
  methods: {
    async getSingleMovie() {
      console.log('url:')
      console.log(this.url)
      // const data = axios.get(`https://api.themoviedb.org/3/movie/${this.$route.params.movieid}?api_key=855c67ea42890d4442543dfe2e92447f&language=en-US`)
      const data = axios.get(`${this.url}/moviesid/${this.$route.params.movieid}`)
      
      const result = await data;
      console.log('single result:')
      console.log(result.data)

      this.movie = result.data;

      console.log('singlemovie:')
      console.log(this.movie)
    },
    
    async getTHProvider() {
      // const data = axios.get(`https://api.themoviedb.org/3/movie/${this.$route.params.movieid}/watch/providers?api_key=855c67ea42890d4442543dfe2e92447f`)
      const data = axios.get(`${this.url}/moviesproth/${this.$route.params.movieid}`)
      
      const result = await data;

      console.log('TH provider result:')
      console.log(result.data.results)
      console.log('message result:')
      console.log(result.data.message)
        
      if(result.data.status !== 404){
        this.THprovider = result.data.results;
        console.log('THprovider:')
        console.log(this.THprovider)
      } else {
        this.nonTHprovider = result.data.message
        console.log('nonTHprovider:')
        console.log(this.nonTHprovider)
      };
    },

    async getENProvider() {
      // const data = axios.get(`https://api.themoviedb.org/3/movie/${this.$route.params.movieid}/watch/providers?api_key=855c67ea42890d4442543dfe2e92447f`)
      const data = axios.get(`${this.url}/moviesprous/${this.$route.params.movieid}`)
      
      const result = await data;

      console.log('EN provider result:')
      console.log(result.data.results)
      console.log('message result:')
      console.log(result.data.message)
        
      if(result.data.status !== 404){
        this.ENprovider = result.data.results;
        console.log('ENprovider:')
        console.log(this.ENprovider)
      } else {
        this.nonENprovider = result.data.message
        console.log('nonENprovider:')
        console.log(this.nonENprovider)
      };
    },
    
  },

}

</script>


<style lang="scss" scoped>
.single-movie {
  color: #fff;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 32px 16px;

  .button {
    align-self: flex-start;
    margin-bottom: 32px;

  }

  .movie-info {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 32px;
    color: #fff;

    @media (min-width: 800px) {
      flex-direction: row;
      align-items: flex-start;
    }

    .movie-img {
      img {
        max-height: 500px;
        width: 100%;

        @media (min-width: 800px) {
          max-height: 700px;
          width: initial;
        }
      }
    }

    .movie-content {
      h1 {
        font-size: 56px;
        font-weight: 400;
      }

      .movie-fact {
        margin-top: 12px;
        font-size: 20px;
        line-height: 1.5;

        span {
          font-weight: 600;
          text-decoration: underline;
        }
      }

      .tagline {
        font-style: italic;

        span {
          font-style: normal;
        }
      }
    }
  }
}

@import url('https://fonts.googleapis.com/css2?family=Karla:wght@200;300;400;500;600&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Karla', sans-serif;
  scroll-behavior: smooth;
}

// app transitions
.page-enter-active,
.page-leave-active {
  transition: opacity 0.5s;
}

.page-enter,
.page-leave-to {
  opacity: 0;
}

.app {
  background-color: #211f1f;
  min-height: 100vh;
}

.button {
  display: inline-block;
  text-decoration: none;
  color: inherit;
  padding: 8px 16px;
  background-color: #007bff;
  border: none;
  color: #fff;
  border-radius: 4px;
  cursor: pointer;
  transition: 0.3s ease all;

  &:hover {
    background-color: #007bff;
  }
}

.button-light {
  background-color: transparent;
  border: 1px solid #007bff;

  &:hover {
    border-color: transparent;
  }
}

.container {
  max-width: 1400px;
  margin: 0 auto;
}

.loading {
  height: 100vh;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;

  @keyframes circle-loading {
    to {
      transform: rotateZ(360deg);
    }
  }

  span {
    display: block;
    width: 70px;
    height: 70px;
    border-radius: 50%;
    border: 2px solid transparent;
    border-top-color: #fff;
    animation: circle-loading 1200ms ease infinite;
  }
}
</style>