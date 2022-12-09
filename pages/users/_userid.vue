<template>
    <div v-if="userRole != 2" class="bg-zinc-800 min-h-screen text-white">
        <FailedPage />
    </div>

    <div v-else class="bg-zinc-800 min-h-screen text-white">
        <b-container>
            <b-row>
                <b-col cols="9" xl="10" lg="10" md="10" sm="10">
                    <SlideBar class="ml-2 pt-20 xs:pl-20 sm:pl-20 md:pl-20 lg:pl-20 xl:pl-20" />
                </b-col>

            </b-row>
        </b-container>

        <div class="mt-20">
            <b-container style="height: auto; width: 1800px;">
                <b-card bg-variant="dark" class="overflow-y-scroll" style="height: 600px;">
                    <div v-if="articles == ''" class="flex justify-center">
                        <p class="font-bold text-4xl">User has no articles.</p>
                    </div>
                    <b-row align-h="around">
                        <div v-for="(a, index ) in articles" :key="index">
                            <b-col cols="12" xl="" lg="12" md="12 mb-4" sm="12" class="">

                                <b-card class="mb-4 px-4 text-xl break-words" style="height: auto; width: 400px;"
                                    bg-variant="dark" text-variant="light" border-variant="primary">
                                    <b-card-text class="text-lg break-words truncate ...">
                                        {{ a.articles }}</b-card-text>
                                    <b-card-text class="text-lg text-[#007bff]"> _________________ </b-card-text>
                                    <b-card-text class="text-sm">Movie name: {{ a.movie_name }}</b-card-text>
                                    <b-card-text class="text-sm">Writer: {{ a.writer }}</b-card-text>
                                    <b-card-text class="text-sm">Date: {{ new
                                            Date(a.date).toLocaleString('en-us', {
                                                month: 'long',
                                                day: 'numeric',
                                                year: 'numeric',
                                            })
                                    }}</b-card-text>

                                    <div class="absolute bottom-3 right-3">
                                        <b-dropdown size="sm" no-caret>
                                            <template #button-content>
                                                <b-icon icon="three-dots-vertical" variant="light" font-scale="1">
                                                </b-icon>
                                            </template>
                                            <b-dropdown-item-button variant="dark" class="text-xs"
                                                @click="deleteArticle(a.article_id)">
                                                <b-icon icon="trash-fill" variant="dark" font-scale="1"
                                                    class="flex justify-end">
                                                </b-icon>
                                                Delete
                                            </b-dropdown-item-button>
                                            <b-dropdown-item-button variant="dark" class="text-xs flex">
                                                <NuxtLink class=""
                                                    :to="{ name: 'articles-articleid', params: { articleid: a.article_id } }">
                                                    <b-icon icon="chat-left-text" font-scale="1">
                                                    </b-icon>
                                                    See comment
                                                </NuxtLink>
                                            </b-dropdown-item-button>
                                        </b-dropdown>
                                    </div>

                                </b-card>

                                <!-- </b-card> -->
                                <!-- </b-container> -->
                            </b-col>
                        </div>
                    </b-row>
                </b-card>
            </b-container>
        </div>
    </div>
</template>

<script>
// import { validationMixin } from "vuelidate";
// import { required, maxLength } from "vuelidate/lib/validators";
// import swal from 'sweetalert2/dist/sweetalert2.js'
import axios from "axios"
import swal from 'sweetalert2/dist/sweetalert2.js';
import SlideBar from '@/components/slide_bar.vue'
import FailedPage from '@/components/failed_page.vue';

export default {
    name: 'UserPage',
    components: {
        SlideBar,
        FailedPage
    },
    // mixins: [validationMixin],
    data() {
        return {
            articles: [],
            // form: {
            //     title: ''
            // },
            singleUser: '',
            userData: '',
            userRole: '',
            editArticleID: '',
            url: 'http://localhost:3000'
            // url: 'https://backend-final.azurewebsites.net'
        }
    },
    // validations: {
    //     form: {
    //         title: {
    //             required,
    //             maxLength: maxLength(1000)
    //         }
    //     }
    // },

    async created() {
        console.log('RedirectUserData1!')

        await this.getSingleUser()
        console.log('UserRole:')
        console.log(this.userRole)
        if (this.userRole != 2) {
            this.$router.push({ name: 'index' })
            console.log('RedirectUserData2!')
            setTimeout(() => {
                swal.fire({
                    title: 'You do not have privileges here.',
                    // text: 'Do you want to continue',
                    icon: 'error',
                    confirmButtonText: 'Done',
                    confirmButtonColor: '#007bff'
                })
            }, 1000);
        } else {
            await this.getArticleByID()
        }
    },

    methods: {
        // GET
        async getSingleUser() {
            try {
                const res = await fetch(this.url + "/getsingleuser", {
                    headers: {
                        'Content-type': 'application/json'
                    },
                    withCredentials: true,
                    credentials: 'include'
                })
                const getuserdata = await res.json()
                this.userData = getuserdata.data
                console.log('UserInfo_page:')
                console.log(this.userData)
                this.userRole = getuserdata.data.role
            }
            catch (error) {
                console.log(`get user failed: ${error}`)
            }
        },

        async getArticleByID() {
            try {
                // const data = axios.get(`${this.url}/getarticlebyidparam/${this.$route.params.userid}`)
                // const result = await data;    
                // this.articles = result.data.data;

                // console.log('_userArticle:')
                // console.log(this.articles)

                const res = await fetch(this.url + "/getarticlebyidparam/" + this.$route.params.userid, {
                    headers: {
                        'Content-type': 'application/json'
                    },
                    credentials: 'include'
                })
                const getarticlebyid = await res.json()
                this.articles = getarticlebyid.data
                console.log('_userArticle:')
                console.log(this.articles)
            }
            catch (error) { console.log(`getArticleUser F: ${error}`) }
        },

        // DELETE
        async deleteArticle(articleId) {
            console.log("Delete ID")
            console.log(articleId)
            try {
                await fetch(`${this.url}/deletearticle/${articleId}`, {
                    method: 'DELETE',
                    credentials: 'include'
                })

                const data = axios.get(`${this.url}/getarticlebyidparam/${this.$route.params.userid}`)
                const result = await data;
                this.singleUser = result.data;

                console.log('RefreshAfterDeleteArticle:')
                console.log(this.singleUser)
            }
            catch (error) {
                console.log(`delete failed: ${error}`)
            }
        },

        onReset(event) {
            event.preventDefault()
            // Reset our form values
            this.editArticleID = ''
            this.form.title = ''
            // Trick to reset/clear native browser form validation state
            this.$nextTick(() => {
                this.$v.$reset();
            });
        },
    }
}
</script>