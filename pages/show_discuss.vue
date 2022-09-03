<template>
  <div class="bg-zinc-800 min-h-screen text-white">
    <b-container fluid>
      <b-row align-h="between">
        <b-col cols="4">
          <SlideBar class="ml-2 pt-20 lg:pl-20 xl:pl-44" />
        </b-col>
        <b-col cols="2">
          <OD class="mr-2 pt-20 lg:pr-20 xl:pr-44" />
        </b-col>
      </b-row>
    </b-container>

    <div class="pt-20 flex">
      <b-container fluid style="max-width: 1800px;">
        <b-row align-h="center">
          <div v-for="(a, index ) in articles" :key="index">
            <b-col>
              <b-card :title="a.articles" tag="article" style="max-width: 28rem; min-width: 20rem; font-size:large"
                class="mb-4" bg-variant="dark" text-variant="light">
                <b-card-text style="font-size:medium">Writer: {{  a.writer  }}</b-card-text>
                <b-card-text style="font-size:medium"> {{
                   new Date(a.date).toLocaleString('en-us', {
                     month: 'long',
                     day: 'numeric',
                     year: 'numeric',
                   })

                  }}</b-card-text>
                <b-card-text style="font-size:medium">Language: {{  a.language  }}</b-card-text>
                <b-row align-h="between">
                  <b-col cols="4">
                    <b-card-text style="font-size:medium">View: {{  a.view  }} </b-card-text>
                    <b-card-text style="font-size:medium">ID: {{  a.article_id  }} </b-card-text>
                  </b-col>

                  <b-col cols="2.5">
                    <b-button>
                      <NuxtLink class="" :to="{ name: 'articles-articleid', params: {articleid: a.article_id} }">
                        <b-icon icon="chat-left-text" variant="primary" font-scale="1"></b-icon>
                      </NuxtLink>
                    </b-button>
                  </b-col>
                </b-row>
              </b-card>
            </b-col>
          </div>
        </b-row>
      </b-container>
    </div>



  </div>
</template>

<script>
import SlideBar from '@/components/slide_bar.vue'
import OD from '@/components/own_discuss.vue'


export default {
  name: 'ShowDiscuss',
  components: {
    SlideBar,
    OD,

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

  methods: {
    async getArticles() {
      try {
        const res = await fetch(this.url + "/getarticle")
        const getarticledata = await res.json()
        // console.log(`getarticledata: ` + getarticledata)
        return getarticledata
      }
      catch (error) { console.log(`get article failed: ${error}`) }
    },
  }


}
</script>