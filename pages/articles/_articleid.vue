<template>

    <div class="bg-zinc-800 min-h-screen text-white">
        <b-container>
            <b-row>
                <b-col cols="9" xl="10" lg="10" md="10" sm="10">
                    <SlideBar class="ml-2 pt-20 xs:pl-20 sm:pl-20 md:pl-20 lg:pl-20 xl:pl-20" />
                </b-col>

            </b-row>
        </b-container>

        <div class="pt-20 flex justify-center" v-for="(a, index ) in article" :key="index">

            <b-col col xl="12" lg="12" md="12" sm="12">
                <b-container style="max-width: 1000px;">
                    
                    <b-card border-variant="primary" bg-variant="secondary">
                        <b-card-text class="text-lg">
                            {{ a.articles }}
                        </b-card-text>
                    </b-card>

                    <b-card border-variant="primary" bg-variant="dark" class="mt-8">
                        <b-row align-h="center" class="w-auto">
                            <b-col col xl="12" lg="12" md="2" sm="12">
                                <b-card :title="a.writer" tag="article" style="" class="mb-2 text-sm"
                                    bg-variant="secondary" text-variant="light" border-variant="primary">
                                    
                                        
                                            <b-card-text style="font-size:medium">: {{ a.text }}</b-card-text>
                                        

                                        <div class="absolute top-4 right-4">
                                                                                    
                                            <b-dropdown size="sm" no-caret>                                            
                                                <template #button-content>
                                                    <b-icon icon="three-dots-vertical" variant="light" font-scale="1">
                                                    </b-icon>
                                                </template>
                                                <b-dropdown-item-button variant="dark" class="text-xs">
                                                    <b-icon icon="trash-fill" variant="dark" font-scale="1"
                                                        class="flex justify-end"></b-icon>
                                                    Delete
                                                </b-dropdown-item-button>
                                            </b-dropdown>                                        
                                        
                                    </div>
                                    
                                </b-card>
                            </b-col>
                        </b-row>
                    </b-card>

                </b-container>
                <div>
                    <CF @comment-data="addComment" class="my-20" />
                </div>
            </b-col>

        </div>


    </div>
</template>
    
    <script>
    import axios from "axios"
    import SlideBar from '@/components/slide_bar.vue'
    import CF from '@/components/comment_form.vue'
    
    export default {
        emits: ['comment-data'],
        name: 'ShowArticle',
        components: {
            SlideBar,
            CF
    
    
        },
        data() {
            return {
                comment: [
                    { writer: "xxx", text: "xxxxx xxxxx xxxxx xxxxx xxxxx", id: 1 },
                    { writer: "xxx", text: "xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx", id: 2 },
                    { writer: "xxx", text: "xxxxx xxxxx xxxxx xxxxx xxxxx", id: 3 },
                    { writer: "xxx", text: "xxxxx xxxxx xxxxx xxxxx xxxxx", id: 4 },
                ],
                article: '',
                url: 'http://localhost:3000'
    
    
            }
        },
        async fetch() {
            await this.getSingleArticle();
        },
        methods: {
            // GET
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
    
            // Post
            async addComment(commentData) {
                console.log('data2')
                console.log(commentData)
                //   const inputData =
                //     JSON.stringify({
                //         articles: discussData.articles,
                //         writer: discussData.writer,
                //         date: discussData.date,
                //         movieName: discussData.movieName,
                //         language: discussData.lang,
                //         view: discussData.view
                //     })
                //   const formData = new FormData()
                //   formData.append('article', inputData)
    
                try {
                    await fetch(this.url + "/addcomment", {
                        method: 'POST',
                        headers: {
                            'Content-type': 'application/json'
                        },
                        body: JSON.stringify({
                            comment: commentData.comment,
                            user_user_id: commentData.userID
                        })
                        //   body: formData
                    })
                    // console.log('data3')
                    // console.log(this.body)
                    // const resdata = await res.json()
                    // this.articles = [...this.articless, resdata]
                }
                catch (error) {
                    console.log(`addArticle False!!! ${error}`)
                }
            },
        }
    }
    </script>