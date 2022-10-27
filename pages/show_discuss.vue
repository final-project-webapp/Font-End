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
      <b-container fluid style="max-width: 2000px;">
        <b-row align-h="center">
          <div v-for="(a, index ) in articles" :key="index">
            <div>
              <b-col cols="12" xl="12" lg="12" md="12" sm="12">

                <b-card :header="a.movie_name" header-text-variant="white" header-bg-variant="dark"
                  header-border-variant="primary" header-tag="header" :title="a.articles" tag="article"
                  style="max-width: 600px; min-width: 200px; min-height:400px; max-width: 600px; font-size:large"
                  class="mb-4 p-4" bg-variant="dark" text-variant="light">
                  <b-card-text class="text-sm">Writer: {{ a.writer }}</b-card-text>
                  <b-card-text class="text-sm"> {{
                  new Date(a.date).toLocaleString('en-us', {
                  month: 'long',
                  day: 'numeric',
                  year: 'numeric',
                  })
                  
                  }}</b-card-text>
                  <b-card-text class="text-sm">Movie name: {{ a.movie_name }}</b-card-text>
                  <b-card-text class="text-sm">Language: {{ a.language }}</b-card-text>
                  <b-card-text class="text-sm">View: {{ a.view }} </b-card-text>
                  <!-- <b-card-text class="text-sm">ID: {{ a.article_id }} </b-card-text> -->
                </b-card>

                <div class="static">
                  <div class="absolute top-3 right-6">
                    <b-dropdown size="sm" no-caret>
                      <template #button-content>
                        <b-icon icon="three-dots-vertical" variant="light" font-scale="1">
                        </b-icon>
                      </template>
                      <b-dropdown-item-button variant="dark" class="px-0 text-xs" @click="deleteArticle(a.article_id)">
                        <b-icon icon="trash-fill" variant="dark" font-scale="1" class="flex justify-end"></b-icon>
                        Delete
                      </b-dropdown-item-button>
                    </b-dropdown>
                  </div>

                  <div class="absolute bottom-3 right-6" v-if="userRole == 1">
                    <b-button @click="countView(a.article_id)">
                      <NuxtLink class="" :to="{ name: 'articles-articleid', params: {articleid: a.article_id} }">
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
              </b-col>
            </div>

          </div>
        </b-row>
      </b-container>
    </div>



  </div>
</template>

<script>
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
      userID: '',
      userData: null,
      userRole: '',
      // url: 'http://localhost:3000'
      url: 'https://backend-final.azurewebsites.net'


    }
  },
  async mounted() {
    if (document.cookie == null) { return }

    try {
      const res = await fetch(this.url + "/getsingleuser", {
        headers: {
          'Content-type': 'application/json'
        },
        credentials: 'include',
        withCredentials: true
      })
      const getuserdata = await res.json()
      this.userData = getuserdata
      console.log('Userdata:')
      console.log(this.userData)
      this.userRole = getuserdata.data.role
      console.log('Userrole:')
      console.log(this.userRole)
      this.userID = getuserdata.data.user_id
      console.log('UserID:')
      console.log(this.userID)

    }
    catch (error) {
      console.log(`get user failed: ${error}`)
    }
  },

  async created() {
    this.articles = await this.getArticles()

    console.log('Articles:')
  },

  methods: {
    // GET
    async getArticles() {
      try {
        const res = await fetch(this.url + "/getarticle")
        const getarticledata = await res.json()
        // console.log(`getarticledata: ` + getarticledata)
        return getarticledata
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
                    body: JSON.stringify({
                        article_id: articleId
                    })
                })
                console.log('countview:')
                console.log(articleId)
            } catch (error) {
                console.log(`countview failed: ${error}`)
            }
        },

    // DELETE
    async deleteArticle(articleId) {
      try {
        await fetch(`${this.url}/deletearticle/${articleId}`, {
          method: 'DELETE',
        })
        this.reloadArticle()
      }
      catch (error) {
        console.log(`delete failed: ${error}`)
      }
    },

    async reloadArticle() {
      this.articles = await this.getArticles()
    }
  }


}
</script>