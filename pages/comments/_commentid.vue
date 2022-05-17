<template>
    <div class="bg-zinc-800 min-h-screen text-white">
        <Loading v-if="$fetchState.pending" />
        <div v-else class="container comment-movie">
            <NuxtLink class="button" :to="{ name: 'index' }">Back</NuxtLink>                       
            <div v-for="(comment, index) in comments" :key="index" class="movie-info">
                <div class="movie-content">
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
            url: 'http://localhost:3000'
        }
    },
    async fetch() {
        await this.getComment();

    },
    fetchDelay: 2000,
    methods: {
        async getComment() {
            try {
                // const data = axios.get(`https://api.themoviedb.org/3/movie/${this.$route.params.commentid}/reviews?api_key=855c67ea42890d4442543dfe2e92447f&language=en-US&page=1`)
                const data = axios.get(`${this.url}/moviesreviews/${this.$route.params.commentid}`)
                const result = await data;
                console.log('comment:')
                console.log(result.data.data.results)
                console.log(this.comments)
                result.data.data.results.forEach((comment) => {
                    this.comments.push(comment)
                })

                console.log('comments:')
                console.log(this.comments)
            }
            catch (error) { console.log(`get comment failed: ${error}`) }
        },

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