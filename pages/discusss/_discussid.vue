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

        <div class="py-20 flex">
            <b-container style="max-width: 1000px;">
                <b-card border-variant="primary" bg-variant="dark">
                    <div class="text-lg">
                        <!-- <p>{{ this.$route.params.discussid }}</p> -->
                        <p>{{ this.form.mname }}</p>
                    </div>
                    <b-form @submit.prevent="onSubmit" @reset="onReset">
                        <b-form-group id="input_group_1" label="Article Title:" label-for="input_1">
                            <b-form-textarea id="input_1" v-model="form.title" type="text" placeholder=""
                                :state="validateState('title')" aria-describedby="feedback_1">
                            </b-form-textarea>
                            <b-form-invalid-feedback id="feedback_1">This is a required field.</b-form-invalid-feedback>
                        </b-form-group>

                        <b-form-group id="input_group_2" label="Writer Name:" label-for="input_2">
                            <b-form-input id="input_2" v-model="form.wname" type="text" placeholder=""
                                :state="validateState('wname')" aria-describedby="feedback_2">
                            </b-form-input>
                            <b-form-invalid-feedback id="feedback_2">This is a required field and must be at
                                least 3 characters.</b-form-invalid-feedback>
                        </b-form-group>

                        <b-form-group id="input_group_3" label="Writing Date:" label-for="input_3">
                            <b-form-input id="input_3" v-model="form.wdate" type="date" placeholder=""
                                :state="validateState('wdate')" aria-describedby="feedback_3">
                            </b-form-input>
                            <b-form-invalid-feedback id="feedback_3">This is a required field.</b-form-invalid-feedback>
                        </b-form-group>

                        <b-form-group id="input_group_5" label="Language:">
                            <b-form-checkbox-group id="checkbox_group_1" v-model="form.lang"
                                :state="validateState('lang')" aria-describedby="feedback_5">
                                <b-form-checkbox value="TH">TH</b-form-checkbox>
                                <b-form-checkbox value="EN">EN</b-form-checkbox>
                            </b-form-checkbox-group>
                            <b-form-invalid-feedback id="feedback_5">This is a required field.</b-form-invalid-feedback>
                        </b-form-group>

                        <div>
                            <div class="absolute bottom-5 left-5">
                                <b-button type="submit" variant="primary" @click="showDismissibleAlert=true">Submit
                                </b-button>
                                <b-button type="reset" variant="danger">Reset</b-button>
                            </div>

                            <div class="flex justify-end">
                                <b-button id="own-discuss" variant="primary" @click="to">
                                    <!-- <nuxt-link class="" :to="{ name: 'comments-commentid' }"> -->
                                    Back
                                    <!-- </nuxt-link> -->
                                </b-button>
                            </div>
                        </div>

                    </b-form>
                </b-card>
                <!-- <div>
                    <b-card class="mt-3" header="Form Data Result">
                        <pre class="m-0">{{ form }}</pre>
                    </b-card>
                </div> -->
            </b-container>
        </div>

        <!-- <DF :moviename="this.namedata" @discuss-data="addArticle" /> -->
        <!-- <h1> Title: {{ this.titleById }} </h1> -->
        <!-- <DF @discuss-data="addArticle" /> -->

    </div>
</template>

<script>
import axios from "axios"
import { validationMixin } from "vuelidate";
import { required, minLength, maxLength } from "vuelidate/lib/validators";
import swal from 'sweetalert2/dist/sweetalert2.js'
import SlideBar from '@/components/slide_bar.vue'

// import DF from '@/components/discuss_form.vue'


export default {
    emits: ['discuss-data'],
    name: 'DiscussPage',
    components: {
        SlideBar,
        // DF
    },
    mixins: [validationMixin],
    data() {
        return {
            form: {
                title: '',
                wname: '',
                wdate: '',
                mname: '',
                lang: [],

            },
            userID: '',
            userData: null,
            userRole: '',
            // titleById: '',
            defaultview: 1,
            testuser: 1,
            showDismissibleAlert: false,
            // articles: [],
            // titleById: '',
            url: 'http://localhost:3000'
            // url: 'https://backend-final.azurewebsites.net'
            // namedata: ''            
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
        }
        catch (error) {
            console.log(`get user failed: ${error}`)
        }
    },
    async fetch() {
        await this.getMovieName();
    },
    validations: {
        form: {
            title: {
                required,
                maxLength: maxLength(1000)
            },
            wname: {
                required,
                minLength: minLength(3),
                maxLength: maxLength(20)
            },
            wdate: {
                required,
            },
            mname: {
                required,
            },
            lang: {
                required
            },
        }
    },
    methods: {
        to() {
            this.$router.go(-1)
        },
        validateState(wname) {
            const { $dirty, $error } = this.$v.form[wname];
            return $dirty ? !$error : null;
        },
        // POST
        async onSubmit() {         
            this.$v.form.$touch();
            if (this.$v.form.$anyError) {
                return;
            }
            try {
                await fetch(this.url + "/addarticle", {
                    method: 'POST',
                    headers: {
                        'Content-type': 'application/json'
                    },
                    body: JSON.stringify({
                        articles: this.form.title,
                        writer: this.form.wname,
                        date: this.form.wdate,
                        movie_name: this.form.mname,
                        language: this.form.lang,
                        view: this.defaultview,
                        user_user_id: this.userID
                    }),
                })
                swal.fire({
                    title: 'Submit Success!',
                    // text: 'Do you want to continue',
                    icon: 'success',
                    confirmButtonText: 'Done',
                    confirmButtonColor: '#007bff'
                })
                setTimeout(() => { this.$router.go(-1) }, 2000);
                console.log('Form:')
                console.log(this.form.title, this.form.wname, this.form.wdate, this.form.mname, this.form.lang, this.defaultview)

                // console.log('data3')
                // console.log(body)
                // const resdata = await res.json()
                // this.articles = [...this.articless, resdata]
            }
            catch (error) {
                console.log(`addArticle False!!! ${error}`)
                swal.fire({
                    title: 'Submit Error!',
                    // text: '${error}',
                    icon: 'error',
                    confirmButtonText: 'Cancel',
                    confirmButtonColor: '#007bff'
                })
                // this.$toast.error('Error while submit')
            }

            // const discussData = {
            //     articles: this.form.title,
            //     writer: this.form.wname,
            //     date: this.form.wdate,
            //     movieName: this.form.mname,
            //     language: this.form.lang,
            //     view: this.defaultview,
            //     userID: this.testuser
            // }
            // this.$emit('discuss-data', discussData)
            // console.log('data:')
            // console.log(discussData)



            // alert(JSON.stringify(this.form))

            this.form.title = ''
            this.form.wname = ''
            this.form.wdate = ''
            this.form.lang = []
            this.$nextTick(() => {
                this.$v.$reset();
            });
        },

        onReset(event) {
            event.preventDefault()
            // Reset our form values
            this.form.title = ''
            this.form.wname = ''
            this.form.wdate = ''
            // this.form.mname = ''
            this.form.lang = []
            // Trick to reset/clear native browser form validation state
            this.$nextTick(() => {
                this.$v.$reset();
            });
        },
        // GET
        async getMovieName() {
            try {
                const dataId = axios.get(`${this.url}/moviessearchId/${this.$route.params.discussid}`)
                console.log('SearchMovieID(Page)')
                console.log(this.$route.params.discussid)

                const resultId = await dataId
                console.log('SearchmoviesName(Page):')
                console.log(resultId.data.data.title)

                // this.titleById = resultId.data.data.title;
                this.form.mname = resultId.data.data.title;

                console.log('titleByIdTest(Page):')
                // console.log(this.titleById)
                console.log(this.form.mname)
            }

            catch (error) { console.log(`get MovieName(Page) failed: ${error}`) }
        },

    }

}


</script>