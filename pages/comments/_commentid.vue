<template>
    <div class="bg-zinc-800 min-h-screen text-white">
        <Loading v-if="$fetchState.pending" />
        <div v-else class="container comment-movie">
            <div class="flex justify-between">
                <NuxtLink class="button" :to="{ name: 'index' }">Back</NuxtLink>


                <NuxtLink v-if="userRole == 1" class="button"
                    :to="{ name: 'discusss-discussid', params: { discussid: this.idById } }">
                    Write your article
                </NuxtLink>

                <div v-if="userData == null" v-b-tooltip.hover.bottom="'Please Login.'">
                    <b-button disabled class="button"> Write your article
                    </b-button>
                    <!-- <b-tooltip target="disabled-wrapper" triggers="hover">Login Please.</b-tooltip> -->
                </div>

            </div>
            <div class="movie-info">
                <div class="movie-content">
                    <h1> Title: {{ this.titleById }} </h1>
                </div>
            </div>

            <b-card bg-variant="dark" class="h-80 overflow-y-scroll pr-2">
                <div v-for="(comment, index) in comments" :key="index" class="movie-info">
                    <div class="movie-content ">
                        <p class="movie-fact">
                        <div class="flex flex-row gap-8">
                            <span class="underline decoration-solid"> {{ comment.author }}</span>
                        </div>
                        <div>
                            <span class="text-sm"> {{ new Date(comment.updated_at).toLocaleString('en-us', {
                                    month: 'long',
                                    day: 'numeric',
                                    year: 'numeric',
                                })
                            }} </span>
                        </div>
                        <div class="h-20 overflow-y-scroll pr-2">
                            <span class="text-xs "> {{ comment.content }}</span>
                        </div>
                        </p>
                    </div>
                </div>
            </b-card>
            <b-card bg-variant="dark" class="h-80 overflow-y-scroll pr-2 mt-8">
                <div class="py-8 flex">
                    <b-container fluid style="max-width: 1800px;">
                        <div v-if="moviearticle == ''" class="flex justify-center">
                            <p class="font-bold text-4xl">No Articles Yet.</p>
                        </div>
                        <b-row align-h="center">
                            <div v-for="(ma, index ) in moviearticle" :key="index">

                                <b-col cols="12" xl="12" lg="12" md="12" sm="12">
                                    <b-card :header="ma.movie_name" header-text-variant="white"
                                        header-border-variant="primary" header-bg-variant="dark" header-tag="header"
                                        :title="ma.articles" tag="article"
                                        style="max-width: 400px; min-width: 200px; min-height:200px; max-height: 400px; font-size:large"
                                        class="pt-8 pl-4 pr-4 pb-4 mb-4" bg-variant="dark" border-variant="primary"
                                        text-variant="light">
                                        <b-card-text class="text-sm">Writer: {{ ma.writer }}</b-card-text>
                                        <b-card-text class="text-sm"> {{
                                                new Date(ma.date).toLocaleString('en-us', {
                                                    month: 'long',
                                                    day: 'numeric',
                                                    year: 'numeric',
                                                })
                                        
                                        }}</b-card-text>
                                        <!-- <b-card-text class="text-sm">Movie name: {{ a.movie_name }}</b-card-text> -->
                                        <b-card-text class="text-sm">Language: {{ ma.language }}</b-card-text>
                                        <b-card-text class="text-sm">View: {{ ma.view }} </b-card-text>
                                        <!-- <b-card-text class="text-sm">WrittterID: {{ ma.user_user_id }} </b-card-text> -->
                                    </b-card>

                                    <div class="static">
                                        <div class="absolute top-3 right-6" v-if="ma.user_user_id === userID">
                                            <b-dropdown size="sm" no-caret>
                                                <template #button-content>
                                                    <b-icon icon="three-dots-vertical" variant="light" font-scale="1">
                                                    </b-icon>
                                                </template>
                                                <b-dropdown-item-button variant="dark" class="px-0 text-xs"
                                                    @click="deleteArticle(ma.article_id)">
                                                    <b-icon icon="trash-fill" variant="dark" font-scale="1"
                                                        class="flex justify-end">
                                                    </b-icon>
                                                    Delete
                                                </b-dropdown-item-button>
                                            </b-dropdown>
                                        </div>

                                        <div class="absolute top-3 right-6" v-if="userRole == 2">
                                            <b-dropdown size="sm" no-caret>
                                                <template #button-content>
                                                    <b-icon icon="three-dots-vertical" variant="light" font-scale="1">
                                                    </b-icon>
                                                </template>
                                                <b-dropdown-item-button variant="dark" class="px-0 text-xs"
                                                    @click="deleteArticle(ma.article_id)">
                                                    <b-icon icon="trash-fill" variant="dark" font-scale="1"
                                                        class="flex justify-end">
                                                    </b-icon>
                                                    Delete
                                                </b-dropdown-item-button>
                                            </b-dropdown>
                                        </div>

                                        <div class="absolute bottom-3 right-6" v-if="userRole == 1">
                                            <b-button @click="countView(ma.article_id)">
                                                <NuxtLink class=""
                                                    :to="{ name: 'articles-articleid', params: { articleid: ma.article_id } }">
                                                    <b-icon icon="chat-left-text" variant="primary" font-scale="1">
                                                    </b-icon>
                                                </NuxtLink>
                                            </b-button>
                                        </div>

                                        <div class="absolute bottom-3 right-6" v-if="userRole == 2">
                                            <b-button @click="countView(ma.article_id)">
                                                <NuxtLink class=""
                                                    :to="{ name: 'articles-articleid', params: { articleid: ma.article_id } }">
                                                    <b-icon icon="chat-left-text" variant="primary" font-scale="1">
                                                    </b-icon>
                                                </NuxtLink>
                                            </b-button>
                                        </div>

                                        <div class="absolute bottom-3 right-6" v-if="userData == null"
                                            v-b-tooltip.hover.bottom="'Please Login.'">
                                            <b-button disabled>
                                                <b-icon icon="chat-left-text" variant="primary" font-scale="1">
                                                </b-icon>
                                            </b-button>
                                            <!-- <b-tooltip target="disabled-wrapper" triggers="hover">Login Please.</b-tooltip> -->
                                        </div>
                                    </div>
                                </b-col>

                            </div>
                        </b-row>
                    </b-container>
                </div>
            </b-card>
        </div>
    </div>
</template>

<script>
import axios from "axios"
import Loading from "../../components/Loading.vue"
export default {
    name: "CommentMovie",
    components: { Loading },
    head() {
        return {
            title: this.$route.params.movietitle
        }
    },
    data() {
        return {
            comments: [],
            moviearticle: [],
            titleById: '',
            idById: '',
            userID: '',
            userData: null,
            userRole: '',
            // namedata: '',
            url: 'https://backend-final.azurewebsites.net'
            // url: 'http://localhost:3000'
        }
    },
    async mounted() {
        console.log('Process 1:')
        console.log(this.userData)
        if (document.cookie == null) { return }

        try {
            console.log('Process 2:')
            const res = await fetch(this.url + "/getsingleuser", {
                headers: {
                    'Content-type': 'application/json'
                },
                credentials: 'include'
            })
            const getuserdata = await res.json()
            this.userData = getuserdata
            console.log('Userdata:')
            console.log(this.userData)
            console.log('Process 3:')
            this.userRole = getuserdata.data.role
            console.log('Userrole:')
            console.log(this.userRole)
            this.userID = getuserdata.data.user_id
            console.log('UserID:')
            console.log(this.userID)
            // return getuserdata

            // MOUNT Articles
            // const data = axios.get(`${this.url}/getsinglearticlename/${this.titleById}`)
            // const result = await data;
            // this.moviearticle = result.data;

            
            // console.log('singlearticle:')
            // console.log(this.moviearticle)
        }
        catch (error) {
            console.log(`get user failed: ${error}`)
        }
    },
    async fetch() {
        await this.getComment();
        await this.getMovieName();
        await this.getSingleArticle();
        await this.getMovieID();

    },
    methods: {
        // GET
        async getSingleArticle() {
            try {
                const data = axios.get(`${this.url}/getsinglearticlename/${this.titleById}`)
            const result = await data;
            this.moviearticle = result.data;

            
            console.log('singlearticle:')
            
            console.log(this.moviearticle)

            }
            catch (error) { console.log(`getSingleArticle: ${error}`) }
        },

        async getComment() {
            console.log('titleById3')
            console.log(this.titleById)
            try {
                const data = axios.get(`${this.url}/moviesreviews/${this.$route.params.commentid}`)
                const result = await data;
                console.log('comment:')
                console.log(result.data.data.results)
                console.log(this.comments)
                result.data.data.results.forEach((comment) => {
                    this.comments.push(comment)
                    console.log('comments:')
                    console.log(this.comments)
                })
            }
            catch (error) { console.log(`get comment failed: ${error}`) }
        },

        async getMovieName() {
            try {
                const dataId = axios.get(`${this.url}/moviessearchId/${this.$route.params.commentid}`)
                console.log('SearchMovieID')
                console.log(this.$route.params.commentid)

                const resultId = await dataId
                console.log('SearchmoviesName:')
                console.log(resultId.data.data.title)

                this.titleById = resultId.data.data.title;

                console.log('titleByIdTest:')
                console.log(this.titleById)

                setTimeout(() => { this.$router.go(0) }, 1000);

                const data = axios.get(`${this.url}/getsinglearticlename/${this.titleById}`)

                const result = await data;
                console.log('single movie:')
                console.log(result.data)

                this.moviearticle = result.data;
                // this.$router.go(0)
            }
            catch (error) { console.log(`get MovieName failed: ${error}`) }
        },

        async getMovieID() {
            try {
                const movieId = axios.get(`${this.url}/moviessearchId/${this.$route.params.commentid}`)
                console.log('SearchID')
                console.log(this.$route.params.commentid)

                const resultId = await movieId
                console.log('SearchmoviesID:')
                console.log(resultId.data.data.id)

                this.idById = resultId.data.data.id;

                console.log('idByIdTest:')
                console.log(this.idById)

            }
            catch (error) { console.log(`get MovieID failed: ${error}`) }
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
            } catch (error) {
                console.log(`countview failed: ${error}`)
            }
        },

        // DELETE
        async deleteArticle(articleId) {
            try {
                await fetch(`${this.url}/deletearticle/${articleId}`, {
                    method: 'DELETE',
                    credentials: 'include',
                })
                const data = axios.get(`${this.url}/getsinglearticlename/${this.titleById}`)

                const result = await data;
                console.log('Delete single movie:')
                console.log(result.data)

                this.moviearticle = result.data;
            }
            catch (error) {
                console.log(`delete failed: ${error}`)
            }
        },

        // async reloadArticle() {
        //     this.moviearticle = await this.getSingleArticle()
        // }


    },

}
</script>

<style lang="scss" scope>
.comment-movie {
    color: #fff;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
    padding: 32px 16px;

    .button {
        align-self: flex-start;
        margin-bottom: 32px;

    }

    .movie-info {
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 32px;
        color: #fff;

        @media (min-width: 800px) {
            flex-direction: row;
            align-items: flex-start;
        }

        .movie-img {
            img {
                max-height: 500px;
                width: 100%;

                @media (min-width: 800px) {
                    max-height: 700px;
                    width: initial;
                }
            }
        }

        .movie-content {
            h1 {
                font-size: 56px;
                font-weight: 400;
            }

            margin-bottom: 24px;

            .movie-fact {
                margin-top: 12px;
                font-size: 20px;
                line-height: 1.5;

                span {
                    text-decoration: underline;
                }
            }

            .tagline {
                font-style: italic;

                span {
                    font-style: normal;
                }
            }
        }
    }
}

@import url('https://fonts.googleapis.com/css2?family=Karla:wght@200;300;400;500;600&display=swap');

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Karla', sans-serif;
    scroll-behavior: smooth;
}

// app transitions
.page-enter-active,
.page-leave-active {
    transition: opacity 0.5s;
}

.page-enter,
.page-leave-to {
    opacity: 0;
}

.app {
    background-color: #211f1f;
    min-height: 100vh;
}

.button {
    display: inline-block;
    text-decoration: none;
    color: inherit;
    padding: 8px 16px;
    background-color: #007bff;
    border: none;
    color: #fff;
    border-radius: 4px;
    cursor: pointer;
    transition: 0.3s ease all;

    &:hover {
        background-color: #007bff;
    }
}

.button-light {
    background-color: transparent;
    border: 1px solid #007bff;

    &:hover {
        border-color: transparent;
    }
}

.container {
    max-width: 1400px;
    margin: 0 auto;
}

.loading {
    height: 100vh;
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;

    @keyframes circle-loading {
        to {
            transform: rotateZ(360deg);
        }
    }

    span {
        display: block;
        width: 70px;
        height: 70px;
        border-radius: 50%;
        border: 2px solid transparent;
        border-top-color: #fff;
        animation: circle-loading 1200ms ease infinite;
    }
}
</style>
