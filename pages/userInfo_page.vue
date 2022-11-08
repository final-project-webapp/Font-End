<template>
    <div v-if="userRole != 1" class="bg-zinc-800 min-h-screen text-white">
        <FailedPage />
    </div>
    <div v-else class="bg-zinc-800 min-h-screen text-white">
        <b-container>
            <b-row align-h="between">
                <b-col cols="5" xl="10" lg="8" md="8" sm="8">
                    <SlideBar class="ml-2 mt-20 xs:ml-20 sm:ml-20 md:ml-20 lg:ml-20 xl:ml-20" />
                </b-col>

                <b-col cols="7" xl="2" lg="4" md="4" sm="4">
                    <UserBotton class="mr-2 mt-20" />
                </b-col>
            </b-row>
        </b-container>

        <div class="mt-20 flex justify-center">
            <b-container style="max-width: 1000px;">
                <!-- <b-card bg-variant="dark" class=""> -->

                <!-- <b-row align-h="center"> -->
                <!-- <b-col cols="10" xl="" lg="12" md="12 mb-4" sm="12" class=""> -->

                <!-- <b-container style="max-width:700px;"> -->

                <!-- <b-card bg-variant="dark" class=""
                                    style="height: 470px; max-width: 700px;"> -->
                <!-- <div v-for="(a, index ) in userData" :key="index"> -->
                <b-card :header="userData.name" header-text-variant="white" header-border-variant="primary"
                    header-bg-variant="dark" header-tag="header" tag="article"
                    class="mb-4 px-4 text-2xl font-bold break-words" style="height: auto; max-width: 1000px;"
                    bg-variant="dark" text-variant="light" border-variant="primary">
                    <b-form @submit.prevent="onSubmit">
                        <b-card-text class="text-xl break-words">Penname:
                        </b-card-text>
                        <b-card-text class="text-base break-words mb-2"> {{ userData.alias }}
                        </b-card-text>
                        <b-form-group id="input_group_1" label-for="input_1">
                            <b-form-input id="input_pname" v-model="form.pname" type="text" placeholder=""
                                :state="validateState('pname')" aria-describedby="feedback_1">
                            </b-form-input>
                            <b-form-invalid-feedback id="feedback_1">This is a required field and must be at
                                least 3 characters.</b-form-invalid-feedback>
                        </b-form-group>

                        <b-card-text class="text-xl break-words">Password:
                        </b-card-text>
                        <b-card-text class="text-base break-words mb-2">{{ userData.password }}
                        </b-card-text>
                        <b-form-group id="input_group_2" label-for="input_2">
                            <b-form-input id="input_pass" v-model="form.password" type="password" placeholder=""
                                rows="8" :state="validateState('password')" aria-describedby="feedback_2">
                            </b-form-input>
                            <b-form-invalid-feedback id="feedback_2">This is a required field and must be at
                                least 8 characters and not more than 24 characters.</b-form-invalid-feedback>
                        </b-form-group>
                        <div class="flex gap-x-2 justify-center">
                        <b-button @click="showUserInfo(userData)" variant="outline-warning">Edit</b-button>
                        <b-button type="submit" variant="outline-primary">Submit</b-button>
                    </div>
                    </b-form>

                    

                    <!-- <div class="absolute bottom-3 right-3">
                        <b-dropdown size="sm" no-caret>
                            <template #button-content>
                                <b-icon icon="three-dots-vertical" variant="light" font-scale="1">
                                </b-icon>
                            </template>
                            <b-dropdown-item-button variant="dark" class="px-0 text-xs" @click="deleteArticle()">
                                <b-icon icon="trash-fill" variant="dark" font-scale="1" class="flex justify-end">
                                </b-icon>
                                Delete
                            </b-dropdown-item-button>
                            <b-dropdown-item-button variant="dark" class="text-xs flex" @click="showArticleInfo()">
                                <b-icon icon="pencil-square" variant="dark" font-scale="1" class="flex justify-end">
                                </b-icon>
                                Edit
                            </b-dropdown-item-button>
                        </b-dropdown>
                    </div> -->

                </b-card>
                <!-- </div> -->
                <!-- </b-card> -->
                <!-- </b-container> -->
                <!-- </b-col> -->

                <!-- <b-col class="mb-4" cols="12" xl="" lg="12" md="12" sm="12">
                            <b-container style="max-width: 700px;">
                                <b-card bg-variant="dark" class="pb-4">
                                    <b-form @submit.prevent="onSubmit" @reset="onReset">
                                        <b-form-group id="input_group_1" label="Article:" label-for="input_1">
                                            <b-form-textarea id="input_1" v-model="form.title" type="text"
                                                placeholder="" :state="validateState('title')"
                                                aria-describedby="feedback_1" rows="8">
                                            </b-form-textarea>
                                            <b-form-invalid-feedback id="feedback_1">This is a required field.
                                            </b-form-invalid-feedback>
                                        </b-form-group>

                                        <div>
                                            <div class="absolute bottom-3 left-5">
                                                <b-button type="submit" variant="primary"
                                                    @click="showDismissibleAlert = true">Submit
                                                </b-button>
                                                <b-button type="reset" variant="danger">Reset</b-button>
                                            </div>
                                        </div>

                                    </b-form>
                                </b-card> -->
                <!-- <div>
                                    <b-card class="mt-3" header="Form Data Result">
                                        <pre class="m-0">{{ form }}</pre>
                                    </b-card>
                                </div> -->
                <!-- </b-container>
                        </b-col> -->
                <!-- </b-row> -->
                <!-- </b-card> -->
            </b-container>
        </div>


    </div>
</template>

<script>
// import axios from "axios"
import { validationMixin } from "vuelidate";
import { required, minLength, maxLength } from "vuelidate/lib/validators";
import swal from 'sweetalert2/dist/sweetalert2.js'
import SlideBar from '@/components/slide_bar.vue'
import UserBotton from '@/components/user_botton.vue'

export default {
    name: 'userInfoPage',
    components: {
        SlideBar,
        UserBotton
    },
    mixins: [validationMixin],
    data() {
        return {
            form: {
                pname: '',
                password: ''
            },
            userData: '',
            userRole: '',
            userInfo: '',
            editArticleID: '',
            url: 'http://localhost:3000'
            // url: 'https://backend-final.azurewebsites.net'
        }
    },
    validations: {
        form: {
            pname: {
                required,
                minLength: minLength(3),
                maxLength: maxLength(20),
            },
            password: {
                required,
                minLength: minLength(8),
                maxLength: maxLength(24)
            },
        }
    },
    async created() {
        console.log('RedirectUserData1!')

        await this.getSingleUser()
        console.log('UserRole:')
        console.log(this.userRole)
        if (this.userRole != 1) {
            this.$router.push({ name: 'index' })
            console.log('RedirectUserData2!')
        }
    },
    methods: {
        validateState(pname) {
            const { $dirty, $error } = this.$v.form[pname];
            return $dirty ? !$error : null;
        },
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
        // EDIT
        showUserInfo(userInfo) {
            this.editUserID = userInfo.user_id
            this.form.pname = userInfo.alias
            this.form.password = userInfo.password
        },
        async onSubmit() {
            this.$v.form.$touch();
            if (this.$v.form.$anyError) {
                return;
            }
            try {
                await fetch(this.url + "/editarticle1", {
                    method: 'PUT',
                    headers: {
                        'Content-type': 'application/json'
                    },
                    credentials: 'include',
                    body: JSON.stringify({
                        article_id: this.editArticleID,
                        articles: this.form.title
                    }),
                })
                const res = await fetch(this.url + "/getarticleowner", {
                    headers: {
                        'Content-type': 'application/json'
                    },
                    credentials: 'include'
                })
                const getuserdata = await res.json()
                this.articles = getuserdata
                console.log('Reloaddata:')
                console.log(this.articles)
                swal.fire({
                    title: 'Submit Success!',
                    // text: 'Do you want to continue',
                    icon: 'success',
                    confirmButtonText: 'Done',
                    confirmButtonColor: '#007bff'
                })
                // setTimeout(() => { this.$router.go(-1) }, 2000);
                // console.log('EditForm:')
                // console.log(this.form.title, this.editArticleID)
                this.editArticleID = ''
                this.form.title = ''
                this.$nextTick(() => {
                    this.$v.$reset();
                });
            }
            catch (error) {
                console.log(`EditArticle False!!! ${error}`)
                swal.fire({
                    title: 'Submit Error!',
                    // text: '${error}',
                    icon: 'error',
                    confirmButtonText: 'Cancel',
                    confirmButtonColor: '#007bff'
                })
            }
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

                const res = await fetch(this.url + "/getarticleowner", {
                    headers: {
                        'Content-type': 'application/json'
                    },
                    credentials: 'include'
                })
                const getuserdata = await res.json()
                this.articles = getuserdata
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