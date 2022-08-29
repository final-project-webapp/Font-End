<template>

    <div class="bg-zinc-800 min-h-screen text-white">
        <b-container fluid>
            <b-row align-h="between">
                <b-col cols="4">
                    <SlideBar class="ml-2 pt-20 lg:pl-20 xl:pl-44" />
                </b-col>

            </b-row>
        </b-container>

        <!-- <div class="pt-20 flex">
            <b-container fluid style="max-width: 1800px;">
                <b-row align-h="center">
                    <div v-for="(c, index ) in comment" :key="index">
                        <b-col lg="">
                            <b-card :title="c.title" tag="article"
                                style="max-width: 28rem; min-width: 20rem; font-size:small" class="mb-4" bg-variant="dark"
                                text-variant="light">
                                <b-card-text style="font-size:medium">Writer: {{ c.text }}</b-card-text> -->
        <!-- <b-row align-h="between">
                                    <b-col cols="4">
                                        <b-card-text style="font-size:medium">View: {{ c.text }} </b-card-text>
                                    </b-col>
    
                                    <b-col cols="2.5">
                                        <b-button>
                                            <NuxtLink class="" :to="{ name: 'discuss-discussid', params: {} }">
                                                <b-icon icon="chat-left-text" variant="primary" font-scale="1"></b-icon>
                                            </NuxtLink>
                                        </b-button>
                                    </b-col>
                                </b-row> -->
        <!-- </b-card>
                        </b-col>
                    </div>
                </b-row>
            </b-container>
        </div> -->


    </div>
</template>
    
    <script>
    import axios from "axios"
    import SlideBar from '@/components/slide_bar.vue'
   
    
    export default {
        name: 'ShowArticle',
        components: {
            SlideBar,
    
    
        },
          data() {
            return {
              comment: [
                {title: "xxx", text: "xxxxx xxxxx", nor: "10" , id: 1},
                {title: "xxx", text: "xxxxx xxxxx", nor: "11" , id: 2},
                {title: "xxx", text: "xxxxx xxxxx", nor: "12" , id: 3},
                {title: "xxx", text: "xxxxx xxxxx", nor: "13" , id: 4}
              ],
              article: '',
              url: 'http://localhost:3000'
    
    
            }
          },
        async fetch() {
            await this.getSingleArticle();
        },
        methods: {
            async getSingleArticle() {
                console.log('url:')
                console.log(this.url)
                // const data = axios.get(`https://api.themoviedb.org/3/movie/${this.$route.params.movieid}?api_key=855c67ea42890d4442543dfe2e92447f&language=en-US`)
                const data = axios.get(`${this.url}/moviesid/${this.$route.params.articleid}`)
    
                const result = await data;
                console.log('single result:')
                console.log(result.data)
    
                this.article = result.data;
    
                console.log('singlearticle:')
                console.log(this.article)
            },
        }
    }
    </script>