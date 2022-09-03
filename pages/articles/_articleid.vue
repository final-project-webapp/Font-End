<template>

    <div class="bg-zinc-800 min-h-screen text-white">
        <b-container fluid>
            <b-row align-h="between">
                <b-col cols="4">
                    <SlideBar class="ml-2 pt-20 lg:pl-20 xl:pl-44" />
                </b-col>

            </b-row>
        </b-container>

        <div class="pt-20 flex" v-for="(a, index ) in article" :key="index">
            <b-container fluid style="max-width: 1200px; min-height: auto;">

                <b-card border-variant="primary" bg-variant="secondary">
                    <b-card-text>
                        {{a.articles}}
                    </b-card-text>
                </b-card>

                <b-card border-variant="primary" bg-variant="dark">
                    
                    <div >
                        <!-- <b-col lg=""> -->
                        <b-row align-h="center" class="">
                        
                            <b-card :title="a.writer" tag="article"
                                style="height: 100px; max-width: 1000px; min-width: 1000px; font-size:small" class="mb-4 overflow-y-scroll"
                                bg-variant="secondary" text-variant="light" border-variant="primary">
                                <b-card-text style="font-size:medium">: {{  a.text  }}</b-card-text>
                                <!-- <b-row>
                                    <b-col cols="4">
                                        <b-card-text style="font-size:medium">: {{  c.id }} </b-card-text>
                                    </b-col> -->

                                    <!-- <b-col cols="2.5">
                                        <b-button>
                                            <NuxtLink class="" :to="{ name: 'discuss-discussid', params: {} }">
                                                <b-icon icon="chat-left-text" variant="primary" font-scale="1"></b-icon>
                                            </NuxtLink>
                                        </b-button>
                                    </b-col> -->
                                <!-- </b-row> -->
                            </b-card>
                        
                        </b-row>
                        <!-- </b-col> -->
                    </div>
                
                </b-card>

                
            </b-container>
        </div>


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
                    { writer: "xxx", text: "xxxxx xxxxx xxxxx xxxxx xxxxx",  id: 1 },
                    { writer: "xxx", text: "xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx",  id: 2 },
                    { writer: "xxx", text: "xxxxx xxxxx xxxxx xxxxx xxxxx",  id: 3 },
                    { writer: "xxx", text: "xxxxx xxxxx xxxxx xxxxx xxxxx",  id: 4 },                    
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
                const data = axios.get(`${this.url}/getsinglearticle/${this.$route.params.articleid}`)
    
                const result = await data;
                console.log('single result:')
                console.log(data)
    
                this.article = result.data;
    
                console.log('singlearticle:')
                console.log(this.article)
            },
        }
    }
    </script>