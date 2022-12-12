<template>
    <div v-if="userRole != 2" class="bg-zinc-800 min-h-screen text-white">
        <FailedPage />
    </div>

    <div v-else class="bg-zinc-800 min-h-screen text-white">
        <NavBar />
        <!-- <b-container>
            <b-row>
                <b-col cols="9" xl="10" lg="10" md="10" sm="10">
                    <SlideBar class="ml-2 pt-20 xs:pl-20 sm:pl-20 md:pl-20 lg:pl-20 xl:pl-20" />
                </b-col>
            </b-row>
        </b-container> -->

        <div class="pt-40">
            <b-container style="height: auto; width: 1800px;">

                <!-- search -->
                <div class="mb-4 flex justify-start">
                    <b-form-input v-model.lazy="searchDummyArticle" type="text" placeholder="Search Movie Name"
                        @keyup.enter="$fetch" @keyup.delete="clearSearch" style="max-width: 400px;"></b-form-input>
                    <b-button v-show="searchInput !== ''" class="ml-2 bg-primary" variant="" size="sm"
                        @click="clearSearch">Clear
                        Search</b-button>
                </div>

                <!-- search article -->
                <div v-if="searchInput !== ''">
                    <b-card bg-variant="dark" class="overflow-y-scroll" style="height: 600px;">
                        <!-- <div class="text-2xl font-bold mb-2 ml-4">
                            {{ userData.name }}
                        </div> -->

                        <div v-if="searchedArticles == ''" class="flex justify-center">
                            <p class="font-bold text-4xl">User has no articles.</p>
                        </div>
                        <b-row align-h="around">
                            <div v-for="(a, index ) in searchedArticles" :key="index">
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
                </div>

                <!-- normal article -->
                <div v-else>
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
                </div>

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
import NavBar from '@/components/nav_bar.vue'
import FailedPage from '@/components/failed_page.vue';

export default {
    name: 'UserPage',
    components: {
        NavBar,
        FailedPage
    },
    // mixins: [validationMixin],
    data() {
        return {
            articles: [],
            singleUser: '',
            userData: '',
            userRole: '',
            editArticleID: '',
            searchDummyArticle: '',
            searchInput: '',
            searchedArticles: [],
            // url: 'http://localhost:3000'
            url: 'https://backend-final.azurewebsites.net'
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
                    icon: 'error',
                    confirmButtonText: 'Done',
                    confirmButtonColor: '#007bff'
                })
            }, 1000);
        } else {
            await this.getArticleByID()
        }
    },

    async fetch() {
        if (this.searchDummyArticle === '') {
            await this.getArticleByID();
        } else {
            await this.searchArticle()
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
                console.log('_userData:')
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

        async searchArticle() {
            this.searchInput = this.searchDummyArticle
            const data = axios.get(`${this.url}/getarticlebymoviename/${this.searchInput}`, { withCredentials: true })
            const result = await data
            console.log('SearchArticle2:')
            console.log(result)
            // result.data.data.forEach((searchArticle) => {
            //     this.searchedArticles.push(searchArticle)
            //     console.log('SearchArticles3:')
            //     console.log(this.searchedArticles)
            // })
        },

        clearSearch() {
            this.searchDummyArticle = ''
            this.searchInput = ''
            this.searchedArticles = []
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