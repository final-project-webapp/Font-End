<template>
  <div class="bg-zinc-800 min-h-screen text-white">
    <!-- <Loading v-if="$fetchState.pending" /> -->
    <b-container fluid>
      <b-row align-h="between">
        <b-col cols="9" xl="10" lg="10" md="10" sm="10">
          <SlideBar class="pt-20 sm:pl-4 md:pl-12 lg:pl-24 xl:pl-48" />
        </b-col>
        <!-- <b-col cols="3" xl="2" lg="2" md="2" sm="2">
          <OD class="pr-2 pt-20" />
        </b-col> -->
      </b-row>
    </b-container>



    <div class="pt-20 flex">
      <b-container fluid style="max-width: 1800px;">
        <div class="mb-4" style="max-width: 400px;">
          <b-form-input v-model.lazy="searchDummyArticle" type="text" placeholder="Enter Movie Name" @keyup.enter="$fetch"
            @keyup.delete="clearSearch"></b-form-input>
          <b-button v-show="searchInput !== ''" class="ml-2 bg-primary" variant="" size="sm" @click="clearSearch">Clear
            Search</b-button>
        </div>

        <b-card bg-variant="dark">
          <b-row align-h="around">
            <b-col cols="2" xl="2" lg="2" md="2" sm="2">
              <div class="object-contain h-auto w-48">
                <img src="@/assests/image/profile1.jpg">
              </div>
            </b-col>
            <b-col cols="10" xl="10" lg="10" md="10" sm="10">


              <!-- <div class="grid grid-cols-2">
            <div class="object-contain h-auto w-48">
              <img src="@/assests/image/profile1.jpg">
            </div>
            <div class="flex justify-start"> -->
              <b-row align-h="around">
                <!-- <div v-for="(a, index ) in articles" :key="index"> -->
                <!-- <div> -->
                <b-col cols="10" xl="10" lg="10" md="10" sm="10">
                  <b-card :header="a" header-text-variant="white" header-bg-variant="dark"
                    header-border-variant="primary" header-tag="header" :title="a" tag="article"
                    style="max-width: 400px; min-width: 200px; min-height:200px; max-width: 400px; font-size:large"
                    class="mb-4 p-4" bg-variant="dark" text-variant="light" border-variant="primary">
                    <b-card-text class="text-sm">Writer: {{ }}</b-card-text>
                    <b-card-text class="text-sm"> {{
                        new Date().toLocaleString('en-us', {
                          month: 'long',
                          day: 'numeric',
                          year: 'numeric',
                        })
                    
                    }}</b-card-text>
                    <b-card-text class="text-sm">Movie name: {{ }}</b-card-text>
                    <b-card-text class="text-sm">Language: {{ }}</b-card-text>
                    <b-card-text class="text-sm">View: {{ }} </b-card-text>
                    <!-- <b-card-text class="text-sm">ID: {{ a.article_id }} </b-card-text> -->
                    <div class="static">
                      <div class="absolute bottom-3 right-6" v-if="userRole == 1">
                        <b-button @click="countView()">
                          <NuxtLink class="" :to="{ name: 'articles-articleid', params: { articleid: a.article_id } }">
                            <b-icon icon="chat-left-text" variant="primary" font-scale="1"></b-icon>
                          </NuxtLink>
                        </b-button>
                      </div>

                      <div class="absolute bottom-3 right-6" v-if="userData == null"
                        v-b-tooltip.hover.bottom="'Please Login.'">
                        <b-button disabled>
                          <b-icon icon="chat-left-text" variant="primary" font-scale="1">
                          </b-icon>
                        </b-button>
                      </div>
                    </div>
                  </b-card>
                </b-col>
                <!-- </div> -->

                <!-- </div> -->
              </b-row>
            </b-col>
          </b-row>
          <!-- </div>
          </div> -->
        </b-card>

        <b-row align-h="around">
          <b-col cols="12" xl="12" lg="12" md="12" sm="12">
            <b-card :header="a" header-text-variant="white" header-bg-variant="dark" header-border-variant="primary"
              header-tag="header" :title="a" tag="article"
              style="max-width: 600px; min-width: 200px; min-height:400px; max-width: 600px; font-size:large"
              class="mb-4 p-4" bg-variant="dark" text-variant="light">
              <b-card-text class="text-sm">Writer: {{ }}</b-card-text>
              <b-card-text class="text-sm"> {{
                  new Date().toLocaleString('en-us', {
                    month: 'long',
                    day: 'numeric',
                    year: 'numeric',
                  })
              
              }}</b-card-text>
              <b-card-text class="text-sm">Movie name: {{ }}</b-card-text>
              <b-card-text class="text-sm">Language: {{ }}</b-card-text>
              <b-card-text class="text-sm">View: {{ }} </b-card-text>
              <!-- <b-card-text class="text-sm">ID: {{ a.article_id }} </b-card-text> -->
            </b-card>
          </b-col>
        </b-row>
      </b-container>
    </div>



  </div>
</template>

<script>
import axios from "axios"
import SlideBar from '@/components/slide_bar.vue'
// import OD from '@/components/own_discuss.vue'

export default {
  name: 'ShowDiscuss',
  components: {
    SlideBar,
    // OD,    
  },
  data() {
    return {
      articles: [],
      searchDummyArticle: '',
      searchInput: '',
      searchedArticles: [],
      userID: '',
      userData: null,
      userRole: '',
      // url: 'http://localhost:3000'
      url: 'https://backend-final.azurewebsites.net'
    }
  },
  // async mounted() {
  //   if (document.cookie == null) { return }

  //   try {
  //     const res = await fetch(this.url + "/getsingleuser", {
  //       headers: {
  //         'Content-type': 'application/json'
  //       },
  //       credentials: 'include'        
  //     })
  //     const getuserdata = await res.json()
  //     this.userData = getuserdata
  //     console.log('Userdata:')
  //     console.log(this.userData)
  //     this.userRole = getuserdata.data.role
  //     console.log('Userrole:')
  //     console.log(this.userRole)
  //     this.userID = getuserdata.data.user_id
  //     console.log('UserID:')
  //     console.log(this.userID)

  //   }
  //   catch (error) {
  //     console.log(`get user failed: ${error}`)
  //   }
  // },

  // async created() {
  //   this.articles = await this.getArticles()

  //   console.log('Articles:')
  // },

  async fetch() {
    // if (this.searchDummyInput === '') {
    //   await this.getMovies()

    // } else {

    //   await this.searchMovies()

    // }
  },

  methods: {
    //   // GET
    //   async getArticles() {
    //     try {
    //       const res = await fetch(this.url + "/getarticle")
    //       const getarticledata = await res.json()
    //       // console.log(`getarticledata: ` + getarticledata)
    //       return getarticledata
    //     }
    //     catch (error) { console.log(`get article failed: ${error}`) }
    //   },

    //   // POST
    //   async countView(articleId) {
    //           try {
    //               await fetch(this.url + "/addview", {
    //                   method: 'POST',
    //                   headers: {
    //                       'Content-type': 'application/json'
    //                   },
    //                   credentials: 'include',
    //                   body: JSON.stringify({
    //                       article_id: articleId
    //                   })
    //               })
    //               this.getArticles()
    //               console.log('countview:')
    //               console.log(articleId)
    //           } catch (error) {
    //               console.log(`countview failed: ${error}`)
    //           }
    //       },
    async searchArticle() {
      console.log('SearchArticles:')
      this.searchInput = this.searchDummyArticle
      const data = axios.get(`${this.url}/searcharticle/${this.searchInput}`)
      const result = await data
      console.log('SearchArticle2:')
      result.data.data.results.forEach((searchArticle) => {
        this.searchedArticles.push(searchArticle)
        console.log('SearchArticles:')
        console.log(this.searchedArticles)
      })
    },

    clearSearch() {
      this.searchDummyArticle = ''
      this.searchInput = ''
      this.searchedArticles = []
    }
  }


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
  }
}