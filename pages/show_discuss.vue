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

                <b-card :header="a.movie_name" header-text-variant="white" header-bg-variant="secondary" header-tag="header" :title="a.articles"
                  tag="article"
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

                  <div class="absolute bottom-3 right-6">
                    <b-button>
                      <NuxtLink class="" :to="{ name: 'articles-articleid', params: {articleid: a.article_id} }">
                        <b-icon icon="chat-left-text" variant="primary" font-scale="1"></b-icon>
                      </NuxtLink>
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
// import Loading from '@/components/Loading.vue'


export default {
  name: 'ShowDiscuss',
  components: {
    SlideBar,
    // OD,
    // Loading

  },
  data() {
    return {
      // discuss: [
      //   { title: "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx", writer: "xxxxx xxxxx", nor: "10", id: 1 },
      //   { title: "xxx", writer: "xxxxx xxxxx", nor: "11", id: 2 },
      //   { title: "xxx", writer: "xxxxx xxxxx", nor: "12", id: 3 },
      //   { title: "xxx", writer: "xxxxx xxxxx", nor: "13", id: 4 }
      // ],
      articles: [],
      url: 'http://localhost:3000'


    }
  },

  async created() {
    this.articles = await this.getArticles()

    console.log('Articles:')
    console.log(this.articles[2])

    // this.userData = await this.getUser();
    // this.userRole = this.userData.data.role

    //  console.log(`type: ${this.userData.data}`)
  },
  // fetchDelay: 2000,
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