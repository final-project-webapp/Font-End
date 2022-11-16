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

            <b-container style="width: 1800px; height: auto;">
                <b-card bg-variant="dark" class="overflow-y-scroll mt-8" style="height: 500px;">
                    <!-- <div v-if="userRole != 2" class="flex justify-center">
                        <p class="font-bold text-4xl">You must be Admin.</p>
                    </div> -->
                    <b-row align-h="around">
                        <div v-for="(au, index ) in allUser" :key="index">

                            <b-col cols="12" xl="12" lg="12" md="12" sm="12">
                                <b-card :header="au.name" header-text-variant="white" header-border-variant="primary"
                                    header-bg-variant="dark" header-tag="header" tag="article"
                                    style="width:380px; height:200px; font-size:large"
                                    class="pt-8 pl-4 pr-4 pb-4 mb-4 break-all" bg-variant="dark"
                                    border-variant="primary" text-variant="light">
                                    <b-card-text class="text-sm">Email: {{ au.emailaddress }}</b-card-text>
                                    <b-card-text class="text-sm"> Birthdate {{
                                            new Date(au.DOB).toLocaleString('en-us', {
                                                month: 'long',
                                                day: 'numeric',
                                                year: 'numeric',
                                            })
                                    }}</b-card-text>
                                </b-card>

                                <div class="static">
                                    <div class="absolute bottom-3 right-6">
                                        <b-dropdown size="sm" no-caret>
                                            <template #button-content>
                                                <b-icon icon="three-dots-vertical" variant="light" font-scale="1">
                                                </b-icon>
                                            </template>
                                            <b-dropdown-item-button variant="dark" class="text-xs"
                                                @click="deleteUser(au.user_id)">
                                                <b-icon icon="trash-fill" variant="dark" font-scale="1"
                                                    class="flex justify-end">
                                                </b-icon>
                                                Delete
                                            </b-dropdown-item-button>
                                            <b-dropdown-item-button variant="dark" class="text-xs flex">
                                                <NuxtLink class=""
                                                    :to="{ name: 'users-userid', params: { userid: au.user_id } }">
                                                    <b-icon icon="info-square-fill" font-scale="1">
                                                    </b-icon>
                                                    Info
                                                </NuxtLink>
                                            </b-dropdown-item-button>
                                        </b-dropdown>
                                    </div>

                                    <!-- <div class="absolute top-3 right-6">
                                        <b-button size="sm">
                                            <NuxtLink class=""
                                                :to="{ name: 'users-userid', params: { userid: au.user_id } }">
                                                <b-icon icon="info" variant="primary" font-scale="1">
                                                </b-icon>
                                            </NuxtLink>
                                        </b-button>
                                    </div> -->
                                </div>
                            </b-col>
                        </div>
                    </b-row>
                </b-card>
            </b-container>

        </div>
    </div>
</template>
<script>
// import axios from "axios"
import SlideBar from '@/components/slide_bar.vue'
import FailedPage from '@/components/failed_page.vue'

export default {
    name: 'ManagePage',
    components: {
        SlideBar,
        FailedPage
    },
    data() {
        return {
            allUser: [],
            userData: null,
            userRole: null,
            // editArticleID: '',
            url: 'http://localhost:3000'
            // url: 'https://backend-final.azurewebsites.net'
        }
    },
    // async fetch() {
    //     await this.getAllUser();

    // },
    async created() {
        console.log('Redirect1!')
        await this.getSingleUser()
        console.log('UserRole:')
        console.log(this.userRole)
        if (this.userRole != 2) {
            this.$router.push({ name: 'index' })
            console.log('Redirect2!')
        } else {
            await this.getAllUser()
        }
    },

    // async mounted() {

    //     if (document.cookie == null) { return }

    //     try {
    //         const res = await fetch(this.url + "/getsingleuser", {
    //             headers: {
    //                 'Content-type': 'application/json'
    //             },
    //             withCredentials: true,
    //             credentials: 'include'
    //         })
    //         const getuserdata = await res.json()
    //         this.userData = getuserdata
    //         console.log('Not login2')

    //         console.log(this.userData)
    //         this.userRole = getuserdata.data.role

    //         console.log(this.userRole)
    //         this.userName = getuserdata.data.name

    //     }
    //     catch (error) {
    //         console.log(`get user failed: ${error}`)
    //     }

    // },

    methods: {
        // GET
        async getSingleUser() {
            // if (document.cookie == null) { return }

            try {
                const res = await fetch(this.url + "/getsingleuser", {
                    headers: {
                        'Content-type': 'application/json'
                    },
                    withCredentials: true,
                    credentials: 'include'
                })
                const getuserdata = await res.json()
                this.userData = getuserdata                

                console.log(this.userData)
                this.userRole = getuserdata.data.role

                console.log(this.userRole)
                this.userName = getuserdata.data.name

            }
            catch (error) {
                console.log(`get user failed: ${error}`)
            }
        },

        async getAllUser() {
            // console.log('Redirect3!')
            // if (this.userRole != 2) {
            //     this.$router.push({ name: 'index' })
            //     console.log('Redirect4!')
            // } else {

            // }
            try {
                // const data = axios.get(`${this.url}/getalluser`)
                // const result = await data;
                // result.data.data.forEach((user) => {
                //     this.allUser.push(user)
                //     console.log('allUser:')
                //     console.log(this.allUser)
                // })               
                const res = await fetch(this.url + "/getalluser")
                const getuserdata = await res.json()
                this.allUser = getuserdata.data
                console.log('Alluser:')
                console.log(this.allUser)
            }
            catch (error) { console.log(`getAllUser: ${error}`) }
        },

        // DELETE
        async deleteUser(userId) {
            console.log('DeleteUserID:')
            console.log(userId)
            try {
                await fetch(`${this.url}/deleteuser/${userId}`, {
                    method: 'DELETE',
                    credentials: 'include'
                })
                // const data = axios.get(`${this.url}/getalluser`)
                // const result = await data;
                // result.data.data.forEach((user) => {
                //     this.allUser.push(user)
                //     console.log('RefreshUser:')
                //     console.log(this.allUser)
                // })
                const res = await fetch(this.url + "/getalluser")
                const getuserdata = await res.json()
                this.allUser = getuserdata.data
                console.log('REAlluser:')
                console.log(this.allUser)
            }
            catch (error) {
                console.log(`delete user failed: ${error}`)
            }
        },
    }
}
</script>