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
        <!-- search -->
        <div class="mb-4 flex justify-start">
          <b-form-input v-model.lazy="searchDummyArticle" type="text" placeholder="Enter Movie Name"
            @keyup.enter="$fetch" @keyup.delete="clearSearch" style="max-width: 400px;"></b-form-input>
          <b-button v-show="searchInput !== ''" class="ml-2 bg-primary" variant="" size="sm" @click="clearSearch">Clear
            Search</b-button>
        </div>

        <!-- Loading -->
        <Loading v-if="$fetchState.pending" />

        <!-- search article -->
        <div v-if="searchInput !== ''">
          <div v-for="(sa, index) in searchedArticles" :key="index">
            {{ sa }}
            <!-- <b-card bg-variant="dark" class="mb-8">

              <b-row align-h="around">
                <b-col cols="2" xl="2" lg="2" md="2" sm="2">
                  <div class="object-contain h-auto w-48">
                    <img :src="`https://image.tmdb.org/t/p/w500/${article.picture_path}`" />
                  </div>
                  <div>
                    <p class="font-bold text-xl justify-center flex mt-4">{{ article.movie }}</p>
                  </div>
                </b-col>
                <b-col cols="10" xl="10" lg="10" md="10" sm="10">

                  <div v-if="(article.articlename == '')">
                    <p class="font-bold text-2xl justify-center flex mt-20">This movie currently has no articles.</p>
                  </div>

                  <b-row align-h="around" class="mt-8">
                    <div v-for="(data, index) in article.articlename" :key="index">
                      <b-col cols="12" xl="12" lg="12" md="12" sm="12">
                        <b-card :header="data.articles" header-text-variant="white" header-bg-variant="dark"
                          header-border-variant="primary" header-tag="header" tag="articles"
                          style="max-width: 400px; min-width: 200px; min-height:200px; max-width: 400px; font-size:large"
                          class="mb-4 p-4" bg-variant="dark" text-variant="light" border-variant="primary">
                          <b-card-text class="text-sm">Writer: {{ data.writer }}</b-card-text>
                          <b-card-text class="text-sm"> {{
                              new Date(data.date).toLocaleString('en-us', {
                                month: 'long',
                                day: 'numeric',
                                year: 'numeric',
                              })
                          }}</b-card-text>
                          <b-card-text class="text-sm">Language: {{ data.language }}</b-card-text>
                          <b-card-text class="text-sm">View: {{ data.view }} </b-card-text>

                          <div class="static">
                            <div class="absolute bottom-3 right-6">
                              <b-button @click="countView(data.article_id)">
                                <NuxtLink class=""
                                  :to="{ name: 'articles-articleid', params: { articleid: data.article_id } }">
                                  <b-icon icon="chat-left-text" variant="primary" font-scale="1"></b-icon>
                                </NuxtLink>
                              </b-button>
                            </div>
                          </div>
                        </b-card>

                      </b-col>

                    </div>
                  </b-row>

                </b-col>
              </b-row>
            </b-card> -->
          </div>
        </div>
        <!-- random article -->
        <div v-else>
          <div v-for="(article, index) in articles" :key="index">
            <b-card bg-variant="dark" class="mb-8">

              <b-row align-h="around">
                <b-col cols="2" xl="2" lg="2" md="2" sm="2">
                  <div class="object-contain h-auto w-48">
                    <img :src="`https://image.tmdb.org/t/p/w500/${article.picture_path}`" />
                  </div>
                  <div>
                    <p class="font-bold text-xl justify-center flex mt-4">{{ article.movie }}</p>
                  </div>
                </b-col>
                <b-col cols="10" xl="10" lg="10" md="10" sm="10">

                  <div v-if="(article.articlename == '')">
                    <p class="font-bold text-2xl justify-center flex mt-20">This movie currently has no articles.</p>
                  </div>

                  <div class="overflow-x-scroll h-80">
                    <b-container style="width: 1600px;">
                      <b-row align-h="start" class="mt-8">
                        <div v-for="(data, index) in article.articlename" :key="index">
                          <b-col cols="12" xl="12" lg="12" md="12" sm="12">
                            <b-card style="max-width: 400px; min-width: 200px; min-height:200px; max-height: 400px;"
                              class="mb-4 p-4 break-words" bg-variant="dark" text-variant="light"
                              border-variant="primary">
                              <b-card-text class="text-lg break-words truncate ...">
                                {{ data.articles }}</b-card-text>
                              <b-card-text class="text-lg text-[#007bff]"> _________________ </b-card-text>
                              <b-card-text class="text-sm">Writer: {{ data.writer }}</b-card-text>
                              <b-card-text class="text-sm"> {{
                                  new Date(data.date).toLocaleString('en-us', {
                                    month: 'long',
                                    day: 'numeric',
                                    year: 'numeric',
                                  })
                              }}</b-card-text>
                              <b-card-text class="text-sm">Language: {{ data.language }}</b-card-text>
                              <b-card-text class="text-sm">View: {{ data.view }} </b-card-text>

                              <div class="static">
                                <div class="absolute top-3 right-6">
                                <b-icon icon="star-fill" variant="success" font-scale="1"></b-icon>                                                                     
                                </div>
                                <div class="absolute bottom-3 right-6">
                                  <b-button @click="countView(data.article_id)">
                                    <NuxtLink class=""
                                      :to="{ name: 'articles-articleid', params: { articleid: data.article_id } }">
                                      <b-icon icon="chat-left-text" variant="primary" font-scale="1"></b-icon>
                                    </NuxtLink>
                                  </b-button>
                                </div>
                              </div>
                            </b-card>

                          </b-col>

                        </div>
                      </b-row>
                    </b-container>
                  </div>

                </b-col>
              </b-row>
            </b-card>
          </div>
        </div>
      </b-container>
    </div>



  </div>
</template>

<script>
import axios from "axios"
import SlideBar from '@/components/slide_bar.vue'
import Loading from '@/components/Loading.vue'

export default {
  name: 'ShowDiscuss',
  components: {
    SlideBar,
    Loading
  },
  data() {
    return {
      articles: [],
      articlesData: [],
      searchDummyArticle: '',
      searchInput: '',
      searchedArticles: [],
      userID: '',
      userData: null,
      userRole: '',
      url: 'http://localhost:3000'
      // url: 'https://backend-final.azurewebsites.net'
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
  //   this.articles = await getRandomArticles()

  //   console.log('Articles:')
  // },

  async fetch() {
    if (this.searchDummyArticle === '') {
      await this.getRandomArticles()

    } else {

      await this.searchArticle()

    }
  },
  fetchDelay: 2000,

  methods: {
    // GET    
    async getRandomArticles() {
      try {
        const data = axios.get(this.url + "/getarticlebymoviename/:page")
        const result = await data

        this.articles = result.data.data
        console.log('Article:')
        console.log(this.articles)

        for (const i in this.articles) {
          // for (const j in this.article[i].articlename) {

          // }
          console.log('ArticleData:')
          console.log(this.articlesData)
          this.articlesData = this.articles[i].articlename
        }
      }
      catch (error) { console.log(`get article failed: ${error}`) }
    },

    // POST
    async countView(articleId) {
      try {
        await fetch(this.url + "/addview", {
          method: 'POST',
          headers: {
            'Content-type': 'application/json'
          },
          credentials: 'include',
          body: JSON.stringify({
            article_id: articleId
          })
        })
        console.log('countview:')
        console.log(articleId)
        await this.getRandomArticles()
      } catch (error) {
        console.log(`countview failed: ${error}`)
      }
    },

    async searchArticle() {
      this.searchInput = this.searchDummyArticle
      const data = axios.get(`${this.url}/searcharticle/${this.searchInput}`)
      const result = await data
      console.log('SearchArticle2:')
      console.log(result.data.data)
      result.data.data.forEach((searchArticle) => {
        this.searchedArticles.push(searchArticle)
        console.log('SearchArticles3:')
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
</style>