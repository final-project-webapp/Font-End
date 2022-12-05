<template>
  <div class="bg-zinc-800 min-h-screen text-white">
    <!-- <Loading v-if="$fetchState.pending" /> -->
    <b-container fluid>
      <b-row align-h="between">
        <b-col cols="4" xl="10" lg="8" md="8" sm="6">
          <SlideBar class="ml-2 mt-20 xs:pl-20 sm:pl-20 md:pl-20 lg:pl-20 xl:pl-40" />
        </b-col>

        <b-col cols="8" xl="2" lg="4" md="4" sm="6">
          <div class="mr-2 mt-20">
            <b-button-group>
              <b-button variant="info" @click="getNewestArticles()">
                Newest
              </b-button>
              <b-button variant="primary" @click="getRandomArticles()">
                Random
              </b-button>
            </b-button-group>
          </div>
        </b-col>
      </b-row>
    </b-container>

    <div class="mt-20 flex">
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
            <b-card bg-variant="dark" class="mb-8">

              <b-row align-h="around">
                <b-col cols="12" xl="2" lg="4" md="4" sm="4">

                  <div class="object-contain h-auto w-48">
                    <img :src="`https://image.tmdb.org/t/p/w500/${sa.picture_path}`" />
                  </div>
                  <div>
                    <NuxtLink
                      :to="{ name: 'comments-commentid', params: { commentid: sa.movie_id, moviename: sa.title } }">
                      <p class="font-bold text-xl justify-center flex mt-4">{{ sa.title }}</p>
                    </NuxtLink>
                  </div>

                </b-col>
                <b-col cols="12" xl="10" lg="8" md="8" sm="12">


                  <div v-if="(sa.articlename == '')">
                    <p class="font-bold text-2xl justify-center flex mt-20">This movie currently has no articles.</p>
                  </div>


                  <b-row align-h="around" class="mt-8">
                    <div v-for="(data, index) in sa.articlename" :key="index">
                      <b-col cols="12" xl="12" lg="12" md="12" sm="12">
                        <b-card style="max-width: 400px; min-width: 200px; min-height:200px; max-height: 400px;"
                          class="mb-4 p-4 break-words" bg-variant="dark" text-variant="light" border-variant="primary">
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
                          <b-card-text class="text-sm">usereID: {{ data.user_user_id }} </b-card-text>

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



                </b-col>
              </b-row>
            </b-card>
          </div>
        </div>

        <!-- random article -->
        <div v-else>
          <div v-for="(article, index) in articles" :key="index">
            <b-card bg-variant="dark" class="mb-8">

              <b-row align-h="around">
                <b-col cols="12" xl="2" lg="4" md="4" sm="4">
                  <div class="object-contain h-auto w-48">
                    <img :src="`https://image.tmdb.org/t/p/w500/${article.picture_path}`" />
                  </div>
                  <div>                   
                    <NuxtLink
                      :to="{ name: 'comments-commentid', params: { commentid: article.movie_id, moviename: article.title } }">
                      <p class="font-bold text-xl justify-center flex mt-4">{{ article.title }}</p>
                    </NuxtLink>                    
                  </div>
                </b-col>

                <b-col cols="12" xl="10" lg="8" md="8" sm="12">
                  <!-- <b-container fluid style="max-width: 1000px;"> -->
                  <div v-if="(article.articlename == '')">
                    <p class="font-bold text-2xl justify-center flex mt-20">This movie currently has no articles.</p>
                  </div>

                  <b-row align-h="around" class="mt-8">
                    <div v-for="(data, index) in article.articlename" :key="index">
                      <b-col cols="12" xl="12" lg="12" md="12" sm="12">
                        <b-card style="max-width: 400px; min-width: auto; min-height:200px; max-height: 400px;"
                          class="mb-4 p-4 break-words" bg-variant="dark" text-variant="light" border-variant="primary">
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
                          <!-- <b-card-text class="text-sm">usereID: {{ data.user_user_id }} </b-card-text>
                          <b-card-text class="text-sm">userRank: {{ rankData }} </b-card-text> -->

                          <div class="static">
                            <div v-for="(r, index) in userRank" :key="index">
                              <!-- <p>R:{{r}}</p> -->

                              <div v-if="(data.user_user_id == r)">
                                <div class="absolute top-3 right-6">
                                  <b-icon icon="star-fill" variant="success" font-scale="1"></b-icon>
                                </div>
                              </div>
                            </div>
                            <!-- </div> -->
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
                  <!-- </b-container> -->


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
      randomArticles: [],
      randomArticlesData: [],
      articles: [],
      articlesData: [],
      searchDummyArticle: '',
      searchInput: '',
      searchedArticles: [],
      userID: '',
      userData: null,
      userRole: '',
      userRank: '',
      rankData: [],
      // url: 'http://localhost:3000'
      url: 'https://backend-final.azurewebsites.net'
    }
  },

  async fetch() {
    if (this.searchDummyArticle === '') {
      await this.getNewestArticles();
      await this.getUserRank();
    } else {
      await this.searchArticle()
      await this.getUserRank();
    }
  },
  fetchDelay: 2000,

  methods: {
    // GET    
    async getNewestArticles() {
      try {
        const data = axios.get(this.url + "/getarticlebypage/1")
        const result = await data

        this.articles = result.data.data
        console.log('Article:')
        console.log(this.articles)

        for (const i in this.articles) {
          // for (const j in this.article[i].articlename) {

          // }
          this.articlesData = this.articles[i].articlename
          console.log('ArticleData:')
          console.log(this.articlesData)

        }
      }
      catch (error) { console.log(`get newest failed: ${error}`) }
    },

    async getRandomArticles() {
      try {
        const data = axios.get(this.url + "/randommoviearticle")
        const result = await data

        this.articles = result.data.data
        console.log('randomArticle:')
        // console.log(this.articles)

        for (const i in this.randomArticles) {
        //   for (const j in this.article[i].articlename) {

        //   }
          this.articlesData= this.articles[i].articlename
          console.log('randomArticleData:')
          console.log(this.articlesData)

        }
      }
      catch (error) { console.log(`get random failed: ${error}`) }
    },

    async getUserRank() {
      try {
        const rank = axios.get(`${this.url}/userrank`)

        const resultRank = await rank
        this.userRank = resultRank.data.user_id
        console.log('UserRank')
        console.log(this.userRank)
        for (const i in this.userRank) {
          this.rankData = this.userRank[i]
        }
        console.log('RankData')
        console.log(this.rankData)
      }
      catch (error) { console.log(`get Rank failed: ${error}`) }
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
        await this.getNewestArticles()
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
      // this.searchedArticles = result.data.data
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