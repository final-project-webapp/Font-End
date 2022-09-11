<template>
    <div class="bg-zinc-800 min-h-screen text-white">
        <Loading v-if="$fetchState.pending" />
        <div v-else class="container comment-movie">
            <div class="flex justify-between">
                <NuxtLink class="button" :to="{ name: 'index' }">Back</NuxtLink>

                <NuxtLink class="button"
                    :to="{ name: 'discuss_page', params: { commentid: this.$route.params.commentid, moviename: this.namedata } }">Write your
                    article</NuxtLink>
            </div>
            <div class="movie-info">
                <div class="movie-content">
                    <h1> Title: {{ this.namedata }} </h1>
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
                    <b-container fluid style="max-width: 1000px;">
                        <b-row align-h="center">
                            <div v-for="(a, index ) in moviearticle" :key="index">
                                <div>
                                    <b-col cols="12" xl="12" lg="12" md="12" sm="12">

                                        <b-card :header="a.movie_name" header-text-variant="white"
                                            header-border-variant="primary" header-bg-variant="dark" header-tag="header"
                                            :title="a.articles" tag="article"
                                            style="max-width: 400px; min-width: 200px; min-height:400px; max-width: 600px; font-size:large"
                                            class="pt-8 pl-4 pr-4 pb-4  mb-4" bg-variant="dark" border-variant="primary"
                                            text-variant="light">
                                            <b-card-text class="text-sm">Writer: {{ a.writer }}</b-card-text>
                                            <b-card-text class="text-sm"> {{
                                            new Date(a.date).toLocaleString('en-us', {
                                            month: 'long',
                                            day: 'numeric',
                                            year: 'numeric',
                                            })
                                            
                                            }}</b-card-text>
                                            <!-- <b-card-text class="text-sm">Movie name: {{ a.movie_name }}</b-card-text> -->
                                            <b-card-text class="text-sm">Language: {{ a.language }}</b-card-text>
                                            <b-card-text class="text-sm">View: {{ a.view }} </b-card-text>
                                            <!-- <b-card-text class="text-sm">ID: {{ a.article_id }} </b-card-text> -->
                                        </b-card>

                                        <div class="static">
                                            <div class="absolute top-3 right-6">
                                                <b-dropdown size="sm" no-caret>
                                                    <template #button-content>
                                                        <b-icon icon="three-dots-vertical" variant="light"
                                                            font-scale="1">
                                                        </b-icon>
                                                    </template>
                                                    <b-dropdown-item-button variant="dark" class="px-0 text-xs"
                                                        @click="deleteArticle(a.article_id)">
                                                        <b-icon icon="trash-fill" variant="dark" font-scale="1"
                                                            class="flex justify-end"></b-icon>
                                                        Delete
                                                    </b-dropdown-item-button>
                                                </b-dropdown>
                                            </div>

                                            <div class="absolute bottom-3 right-6">
                                                <b-button>
                                                    <NuxtLink class=""
                                                        :to="{ name: 'articles-articleid', params: {articleid: a.article_id} }">
                                                        <b-icon icon="chat-left-text" variant="primary" font-scale="1">
                                                        </b-icon>
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
            // movies: [],
            moviearticle: [],
            namedata: '',           
            url: 'https://backend-final.azurewebsites.net',
            // url: 'http://localhost:3000'
        }
    },
    async fetch() {
        await this.getComment();
        await this.getSingleArticle();
        await this.getMovieName();

    },
    // fetchDelay: 2000,
    methods: {
        async getSingleArticle() {
            try {
                // console.log('url:')
                console.log('namedata2')
                console.log(this.namedata)
                console.log(this.comments)
                // const data = axios.get(`https://api.themoviedb.org/3/movie/${this.$route.params.movieid}?api_key=855c67ea42890d4442543dfe2e92447f&language=en-US`)
                const data = axios.get(`${this.url}/getsinglearticlename/${this.test}`)

                const result = await data;
                console.log('single movie:')
                console.log(result)

                this.moviearticle = result.data;

                console.log('singlearticle:')
                console.log(this.moviearticle)
            }
            catch (error) { console.log(`getSingleArticle: ${error}`) }
        },

        async getComment() {
            console.log('namedata3')
            console.log(this.namedata)
            try {
                // const data = axios.get(`https://api.themoviedb.org/3/movie/${this.$route.params.commentid}/reviews?api_key=855c67ea42890d4442543dfe2e92447f&language=en-US&page=1`)
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


                // const dataId = axios.get(`${this.url}/moviessearchId/${this.$route.params.commentid}`)
                // console.log('SearchMovieID')
                // console.log(this.$route.params.commentid)

                // const resultId = await dataId
                // console.log('Searchmovies2:')
                // console.log(this.dataId)
                // resultId.data.title.forEach((movietitle) => {
                //     this.Moviename2.push(movietitle)

                // })

                // console.log('cid')
                // console.log(this.$route.params.commentid)
                // console.log('mname')
                // console.log(this.$route.params.moviename)

            }
            catch (error) { console.log(`get comment failed: ${error}`) }
        },

        async getMovieName() {
            try {
                const dataId = axios.get(`${this.url}/moviessearchId/${this.$route.params.commentid}`)
                console.log('SearchMovieID')
                console.log(this.$route.params.commentid)

                const resultId = await dataId
                console.log('Searchmovies2:')
                console.log(resultId.data.data)

                this.namedata = resultId.data.data.title;
                


                console.log('NameData:')
                console.log(this.namedata)

                const data = axios.get(`${this.url}/getsinglearticlename/${this.namedata}`)

                const result = await data;
                console.log('single movie:')
                console.log(result)

                this.moviearticle = result.data;


                // console.log('cid')
                // console.log(this.$route.params.commentid)
                // console.log('mname')
                // console.log(this.$route.params.moviename)

            }

            catch (error) { console.log(`get MovieName failed: ${error}`) }
        },
        // async getMovies() {
        //     console.log(this.url)
        //     try {
        //         // const data = axios.get(`https://api.themoviedb.org/3/movie/now_playing?api_key=855c67ea42890d4442543dfe2e92447f&language=en-US&page=1`)
        //         const data = axios.get(this.url + "/movies/1")

        //         const result = await data

        //         console.log('movie:')
        //         console.log(result.data.data.results)
        //         console.log('movies:')

        //         result.data.data.results.forEach((movie) => {
        //             this.movies.push(movie)
        //         })
        //         console.log(this.movies)
        //     }
        //     catch (error) { console.log(`get movie failed: ${error}`) }
        // },


        // GET
        // async getSingleArticle() {
        //     try {
        //     // console.log('url:')
        //     console.log('namedata2')
        //     console.log(this.namedata)
        //     console.log(this.comment)
        //     // const data = axios.get(`https://api.themoviedb.org/3/movie/${this.$route.params.movieid}?api_key=855c67ea42890d4442543dfe2e92447f&language=en-US`)
        //     const data = axios.get(`${this.url}/getsinglearticlename/${this.namedata}`)

        //     const result = await data;
        //     console.log('single movie:')
        //     console.log(result)

        //     this.moviearticle = result.data;

        //     console.log('singlearticle:')
        //     console.log(this.moviearticle)
        // }
        //     catch (error) { console.log(`getSingleArticle: ${error}`) }
        // },

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