<template>

    <div class="bg-zinc-800 min-h-screen text-white">
        <b-container>
            <b-row>
                <b-col cols="9" xl="10" lg="10" md="10" sm="10">
                    <SlideBar class="ml-2 pt-20 xs:pl-20 sm:pl-20 md:pl-20 lg:pl-20 xl:pl-20" />
                </b-col>

            </b-row>
        </b-container>

        <div class="pt-20 flex justify-center">

            <b-col col xl="12" lg="12" md="12" sm="12">
                <b-container style="max-width: 1000px;">

                    <div v-for="(a, index ) in article" :key="index">
                        <b-card border-variant="primary" bg-variant="secondary">
                            <b-card-text class="text-lg">
                                {{ a.articles }}
                            </b-card-text>
                            <b-card-text class="text-lg">
                                {{ a.article_id }}
                            </b-card-text>
                        </b-card>
                    </div>

                    <b-card border-variant="primary" bg-variant="dark" class="mt-8 overflow-y-scroll h-64">
                        <b-row align-h="center" class="w-auto">
                            <b-col col xl="12" lg="12" md="2" sm="12">
                                <div v-for="ac in articleComment" :key="ac.article_id">
                                    <b-card :title="ac.name" tag="article" style="" class="mb-2 text-xs"
                                        bg-variant="secondary" text-variant="light" border-variant="primary">


                                        <b-card-text class="text-sm">: {{ ac.comment }}</b-card-text>
                                        <b-card-text class="text-sm">ID: {{ ac.comment_id }}</b-card-text>

                                        <div class="absolute top-4 right-4">

                                            <b-dropdown size="sm" no-caret>
                                                <template #button-content>
                                                    <b-icon icon="three-dots-vertical" variant="light" font-scale="1">
                                                    </b-icon>
                                                </template>
                                                <b-dropdown-item-button variant="dark" class="text-xs"
                                                    @click="deleteComment(ac.comment_id)">
                                                    <b-icon icon="trash-fill" variant="dark" font-scale="1"
                                                        class="flex justify-end"></b-icon>
                                                    Delete
                                                </b-dropdown-item-button>
                                            </b-dropdown>

                                        </div>

                                    </b-card>
                                </div>
                            </b-col>
                        </b-row>
                    </b-card>

                </b-container>
                <div>
                    <CF class="mt-20" @comment-data="addComment" />
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
    name: 'ShowArticle',
    components: {
        SlideBar,
        CF
    },
    emits: ['comment-data'],


    data() {
        return {
            comment: [
                { writer: "xxx", text: "xxxxx xxxxx xxxxx xxxxx xxxxx", id: 1 },
                { writer: "xxx", text: "xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx xxxxx", id: 2 },
                { writer: "xxx", text: "xxxxx xxxxx xxxxx xxxxx xxxxx", id: 3 },
                { writer: "xxx", text: "xxxxx xxxxx xxxxx xxxxx xxxxx", id: 4 },
            ],
            article: '',
            articleComment: [],
            articleId: '',
            url: 'http://localhost:3000'
            // url: 'https://backend-final.azurewebsites.net'
            


        }
    },
    async fetch() {
        await this.getSingleArticle();
        await this.getCommentArticle()
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

        async getCommentArticle() {
            // console.log('articleId')
            // console.log(this.$route.params.articleid)
            // console.log('url2:')
            // console.log(this.url)
            // const data = axios.get(`https://api.themoviedb.org/3/movie/${this.$route.params.movieid}?api_key=855c67ea42890d4442543dfe2e92447f&language=en-US`)
            const data = axios.get(`${this.url}/getcommentinarticle/${this.$route.params.articleid}`)

            const result = await data;
            // console.log('Comment Article:')
            // console.log(data.JSON)

            this.articleComment = result.data;
            this.articleId = this.$route.params.articleid

            console.log('Comment Article:')
            console.log(this.articleComment)
        },

        // Post
        async addComment(commentData) {
            try {
                console.log('POST')
                await fetch(this.url + "/addcomment", {
                    method: 'POST',
                    headers: {
                        'Content-type': 'application/json'
                    },
                    body: JSON.stringify({
                        comment: commentData.comment,
                        user_user_id: commentData.user_user_id,
                        article_user_user_id: this.article[0].user_user_id,
                        article_article_id: this.$route.params.articleid
                    })
                })
                // console.log('article_user_user_id')
                // console.log(this.article[0].user_user_id)
                // console.log('RESdata') 
                // console.log(res.data) 
                // const resdata = await res.json()

                // this.articleComment = [...this.articleComment, resdata]

                // console.log('First ArticleComment') 
                // console.log(this.articleComment)
                // this.articleComment = await this.getCommentArticle()  
                // console.log(this.article[0].user_user_id)
                // console.log('Second ArticleComment') 
                // console.log(this.articleComment)
                console.log('Post Your comment success')
                const data = axios.get(`${this.url}/getcommentinarticle/${this.articleId}`)
                const result = await data;
                console.log('Result')
                console.log(result)

                this.articleComment = result.data;
            }
            catch (error) {
                console.log(`addArticle False!!! ${error}`)
            }
        },

        // DELETE
        async deleteComment(commentId) {
            try {
                await fetch(`${this.url}/deletecomment/${commentId}`, {
                    method: 'DELETE',
                })
                const data = axios.get(`${this.url}/getcommentinarticle/${this.articleId}`)
                const result = await data;
                console.log('DeleteResult')
                console.log(result)

                this.articleComment = result.data;
                // await this.getCommentArticle()
                console.log('Your comment delete success')
                // alert('Delete Success')
            }
            catch (error) {
                console.log(`delete failed: ${error}`)
            }
        },

        async reloadComment() {
            this.articleComment = await this.getCommentArticle()
        }
    }
}
</script>