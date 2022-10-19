<template>
    <div class="bg-zinc-800 min-h-screen text-white">
        <b-container>
            <b-row align-h="between">
                <b-col cols="8" xl="11" lg="10" md="10" sm="9">
                    <SlideBar class="ml-2 mt-20 xs:ml-20 sm:ml-20 md:ml-20 lg:ml-20 xl:ml-20" />
                </b-col>
                <b-col cols="4" xl="1" lg="2" md="2" sm="3">
                    <b-button variant="primary" class="mr-2 mt-20">
                        <nuxt-link class="" :to="{ name: 'userHistory_page' }">
                            History
                        </nuxt-link>
                    </b-button>
                </b-col>
            </b-row>
        </b-container>

        <div class="mt-20">
            <b-container>
                <b-row align-h="center">
                    <b-col class="mb-4">
                        <b-container style="min-width:500px;">
                            <b-card :header="userName" header-text-variant="white" header-border-variant="primary"
                                header-bg-variant="dark" header-tag="header" tag="article" class="px-4 text-xl"
                                style="height: 525px; width: auto;" bg-variant="dark" text-variant="light"
                                border-variant="primary">
                                <b-card-text class="text-lg mt-2">Email: {{emailaddress }}</b-card-text>
                                <b-card-text class="text-lg mt-2">Birthdate: {{new Date(date).toLocaleString('en-us', {
                                month: 'long',
                                day: 'numeric',
                                year: 'numeric',
                                }) }}</b-card-text>

                                <div class="absolute top-3 right-6">
                                    <b-dropdown size="sm" no-caret>
                                        <template #button-content>
                                            <b-icon icon="three-dots-vertical" variant="light" font-scale="1">
                                            </b-icon>
                                        </template>
                                        <b-dropdown-item-button variant="dark" class="text-xs flex"
                                            @click="showUserInfo(userInfo)">
                                            <b-icon icon="pencil-square" variant="dark" font-scale="1"
                                                class="flex justify-end">                                                
                                            </b-icon>
                                            Edit
                                        </b-dropdown-item-button>
                                    </b-dropdown>
                                </div>

                            </b-card>
                        </b-container>
                    </b-col>
                    <b-col class="mb-4">
                        <b-container style="min-width: 500px;">
                            <b-card border-variant="primary" bg-variant="dark">
                                <b-form @submit.prevent="onSubmit">
                                    <b-form-group id="input_group_1" label="Name:" label-for="input_1">
                                        <b-form-input id="input_name" v-model="form.uname" type="text" placeholder=""
                                            :state="validateState('uname')" aria-describedby="feedback_1">
                                        </b-form-input>
                                        <b-form-invalid-feedback id="feedback_1">This is a required field and must be at
                                            least 3 characters and not more than 20 characters.
                                        </b-form-invalid-feedback>
                                    </b-form-group>

                                    <b-form-group id="input_group_2" label="Email:" label-for="input_2">
                                        <b-form-input id="input_email" v-model="form.emailaddress" type="email"
                                            placeholder="" :state="validateState('emailaddress')"
                                            aria-describedby="feedback_2">
                                        </b-form-input>
                                        <b-form-invalid-feedback id="feedback_2">This is a required field.
                                        </b-form-invalid-feedback>
                                    </b-form-group>

                                    <!-- <b-form-group id="input_group_3" label="Password:" label-for="input_3">
                                        <b-form-input id="input_pass" v-model="form.password" type="password"
                                            placeholder="" rows="8" :state="validateState('password')"
                                            aria-describedby="feedback_3">
                                        </b-form-input>
                                        <b-form-invalid-feedback id="feedback_3">This is a required field and must be at
                                            least 8 characters and not more than 24 characters.
                                        </b-form-invalid-feedback>
                                    </b-form-group> -->

                                    <b-form-group id="input_group_3" label="DOB:" label-for="input_3">
                                        <b-form-input id="input_DOB" v-model="form.date" type="date" placeholder=""
                                            rows="8" :state="validateState('date')" aria-describedby="feedback_3">
                                        </b-form-input>
                                        <b-form-invalid-feedback id="feedback_3">This is a required field.
                                        </b-form-invalid-feedback>
                                    </b-form-group>
                                    <div class="justify-center flex">
                                        <b-button type="submit" variant="primary">Submit</b-button>
                                        <!-- <b-button type="reset" variant="danger">Reset</b-button> -->
                                    </div>
                                </b-form>
                            </b-card>
                            <!-- <div>
                            <b-card class="mt-3" header="Form Data Result">
                                <pre class="m-0">{{ form }}</pre>
                            </b-card>
                        </div> -->
                        </b-container>
                    </b-col>
                </b-row>
            </b-container>
        </div>
    </div>
</template>

<script>
import { validationMixin } from "vuelidate";
import { required, minLength, maxLength, email } from "vuelidate/lib/validators";
import SlideBar from '@/components/slide_bar.vue'

export default {
    name: 'ManagePage',
    components: {
        SlideBar
    },
    mixins: [validationMixin],
    data() {
        return {
            form: {
                uname: '',
                emailaddress: '',
                // password: '',
                date: ''
            },
            name: 'yuzuriha_weiR',
            userData: '',
            userInfo:'',
            userName: '',
            date: '',
            emailaddress: '',
            editUserID:'',
            mail: 'mail@mail.com',
            url: 'http://localhost:3000'
            // url: 'https://backend-final.azurewebsites.net'
        }
    },
    validations: {
        form: {
            uname: {
                required,
                minLength: minLength(3),
                maxLength: maxLength(20),
            },
            emailaddress: {
                required,
                email
            },
            // password: {
            //     required,
            //     minLength: minLength(8),
            //     maxLength: maxLength(24)
            // },
            date: {
                required
            },
        }
    },
    async mounted() {
        if (document.cookie == null) { return }
        try {
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
            this.userInfo = getuserdata.data
            console.log('UserInfo:')
            console.log(this.userInfo)
            this.date = getuserdata.data.DOB
            console.log('DOB:')
            console.log(this.date)
            this.userName = getuserdata.data.name
            console.log('Username:')
            console.log(this.userName)
            this.emailaddress = getuserdata.data.emailaddress
            console.log('Email:')
            console.log(this.emailaddress)
            // return getuserdata
        }
        catch (error) {
            console.log(`get user failed: ${error}`)
        }
    },
    methods: {
        validateState(uname) {
            const { $dirty, $error } = this.$v.form[uname];
            return $dirty ? !$error : null;
        },
        onSubmit() {
            this.$v.form.$touch();
            if (this.$v.form.$anyError) {
                return;
            }

            const registerData = {
                userName: this.form.uname,
                emailAddress: this.form.emailaddress,
                // password: this.form.password,
                dob: this.form.date
            }
            this.$emit('register-user', registerData)
            console.log('regisdata:')
            console.log(registerData)

            this.form.uname = ''
            this.form.emailaddress = ''
            // this.form.password = ''
            this.form.date = ''
            this.$nextTick(() => {
                this.$v.$reset();
            });

        },
        // EDIT
        showUserInfo(userInfo) {
            this.editUserID = userInfo.user_id
            this.form.uname = userInfo.name
            this.form.emailaddress = userInfo.emailaddress
            this.form.date = userInfo.DOB
        },


    }
}
</script>