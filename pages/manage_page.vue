<template>
    <div class="bg-zinc-800 min-h-screen text-white">
        <b-container>
            <b-row>
                <b-col cols="9" xl="10" lg="10" md="10" sm="10">
                    <SlideBar class="ml-2 pt-20 xs:pl-20 sm:pl-20 md:pl-20 lg:pl-20 xl:pl-20" />
                </b-col>

            </b-row>
        </b-container>

        <div class="mt-20">

            <b-container style="max-width: 1800px; height: 470px;">
                <b-card bg-variant="dark" class="overflow-y-scroll pr-2 mt-8"
                style="height: 470px;">
                    <b-row align-h="center">
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
                                    <div class="absolute top-3 right-6">
                                        <b-dropdown size="sm" no-caret>
                                            <template #button-content>
                                                <b-icon icon="three-dots-vertical" variant="light" font-scale="1">
                                                </b-icon>
                                            </template>
                                            <b-dropdown-item-button variant="dark" class="px-0 text-xs"
                                                @click="deleteUser(au.user_id)">
                                                <b-icon icon="trash-fill" variant="dark" font-scale="1"
                                                    class="flex justify-end">
                                                </b-icon>
                                                Delete
                                            </b-dropdown-item-button>
                                        </b-dropdown>
                                    </div>

                                    <div class="absolute bottom-3 right-6">
                                        <b-button size="sm">
                                            <NuxtLink class=""
                                                :to="{ name: 'users-userid', params: { userid: au.user_id } }">
                                                <b-icon icon="info" variant="primary" font-scale="1">
                                                </b-icon>
                                            </NuxtLink>
                                        </b-button>
                                    </div>
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

export default {
    name: 'ManagePage',
    components: {
        SlideBar
    },
    data() {
        return {
            allUser: [],
            // userData: '',
            // userInfo: '',
            // editArticleID: '',
            // url: 'http://localhost:3000'
            url: 'https://mediare.azurewebsites.net'
        }
    },
    // async fetch() {
    //     await this.getAllUser();

    // },
    async created() {
        await this.getAllUser()
    },

    methods: {
        // GET
        async getAllUser() {
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