<template>
    <div class="bg-zinc-800 min-h-screen text-white">
        <b-container fluid>
            <b-row align-h="between">
                <b-col cols="4">
                    <SlideBar class="ml-2 pt-20 lg:pl-20 xl:pl-44" />
                </b-col>
                <b-col cols="2">

                </b-col>
            </b-row>
        </b-container>

        <!-- <DF :moviename="this.namedata" @discuss-data="addArticle" /> -->
        <DF @discuss-data="addArticle" />

    </div>
</template>

<script>
// import axios from "axios"
import swal from 'sweetalert2/dist/sweetalert2.js'

import SlideBar from '@/components/slide_bar.vue'
import DF from '@/components/discuss_form.vue'


export default {
    emits: ['discuss-data'],
    name: 'DiscussPage',
    components: {
        SlideBar,
        DF

    },
    data() {
        return {
            // articles: [],
            url: 'http://localhost:3000'
            // url: 'https://backend-final.azurewebsites.net'
            // namedata: ''            
        }
    },
    // async fetch() {        
    //     await this.getMovieName();

    // },
    methods: {
        // GET
        // async getMovieName() {
        //     try {
        //         const dataId = axios.get(`${this.url}/moviessearchId/${this.$route.params.commentid}`)
        //         console.log('SearchMovieID')
        //         console.log(this.$route.params.commentid)

        //         const resultId = await dataId
        //         console.log('Searchmovies2:')
        //         console.log(resultId.data.data)

        //         this.namedata = resultId.data.data.title;



        //         console.log('NameData Dis:')
        //         console.log(this.namedata)



        //         // this.$emit('name-data', namedata)

        //         // console.log('cid')
        //         // console.log(this.$route.params.commentid)
        //         // console.log('mname')
        //         // console.log(this.$route.params.moviename)

        //     }

        //     catch (error) { console.log(`get MovieName failed: ${error}`) }
        // },

        // Post
        async addArticle(discussData) {
            console.log('data2')
            console.log(discussData)
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
                await fetch(this.url + "/addarticle", {
                    method: 'POST',
                    headers: {
                        'Content-type': 'application/json'
                    },
                    body: JSON.stringify({
                        articles: discussData.articles,
                        writer: discussData.writer,
                        date: discussData.date,
                        movie_name: discussData.movieName,
                        language: discussData.language,
                        view: discussData.view,
                        user_user_id: discussData.userID
                    }),
                    //   body: formData
                })
                // this.$toast.success('Successfully submit')
                swal.fire({
                    title: 'Submit Success!',
                    // text: 'Do you want to continue',
                    icon: 'success',
                    confirmButtonText: 'Done'
                })

                // console.log('data3')
                // console.log(body)
                // const resdata = await res.json()
                // this.articles = [...this.articless, resdata]
            }
            catch (error) {
                console.log(`addArticle False!!! ${error}`)
                // this.$toast.error('Error while submit')
            }

        },
    }

}


</script>