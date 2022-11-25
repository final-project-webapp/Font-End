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
                    <b-form @submit.prevent="confirmPenname">
                        <div class="flex justify-between">
                            <b-card-text class="text-xl break-words">Pen name:
                            </b-card-text>
                            <b-button size="sm" v-show="!editPenname" @click="showPenname(userData); togglePenname();"
                                variant="outline-warning">Edit Penname</b-button>
                            <b-button size="sm" v-show="editPenname" @click="hidePenname(); togglePenname();"
                                variant="outline-danger">Cancel</b-button>
                        </div>


                        <b-card-text class="text-base break-words mb-2"> {{ userData.alias }}
                        </b-card-text>
                        <div v-show="editPenname">
                            <b-form-group id="input_group_1" label-for="input_1">
                                <b-form-input id="input_pname" v-model="pname" type="text" placeholder=""
                                    aria-describedby="feedback_1">
                                </b-form-input>
                                <!-- <b-form-invalid-feedback id="feedback_1">This is a required field and must be at
                                    least 3 characters.</b-form-invalid-feedback> -->
                            </b-form-group>
                        </div>
                        <div class="flex justify-center mt-2 mb-4" v-show="editPenname">
                            <b-button type="submit" variant="outline-primary">Submit</b-button>
                        </div>
                    </b-form>

                    <b-form @submit.prevent="confirmPassword">

                        <div class="flex justify-between">
                            <b-card-text class="text-xl break-words">Password:
                            </b-card-text>
                            <b-button size="sm" v-show="!editPassword" @click="togglePassword();"
                                variant="outline-warning">
                                Edit
                                Password</b-button>
                            <b-button size="sm" v-show="editPassword" @click="hidePassword(); togglePassword();"
                                variant="outline-danger">Cancel</b-button>
                        </div>


                        <b-card-text class="text-base break-words mb-2">********
                        </b-card-text>
                        <div v-show="editPassword">
                            <b-form-group id="input_group_2" label-for="input_2">
                                <b-form-input id="input_oldpass" v-model="form.oldpassword" type="password"
                                    placeholder="Current password" rows="8" :state="validateState('oldpassword')"
                                    aria-describedby="feedback_2">
                                </b-form-input>
                                <b-form-invalid-feedback id="feedback_2" class="text-sm">This is a required field and
                                    must be at
                                    least 8 characters and not more than 24 characters.</b-form-invalid-feedback>
                            </b-form-group>
                            <b-form-group id="input_group_3" label-for="input_3">
                                <b-form-input id="input_newpass" v-model="form.newpassword" type="password"
                                    placeholder="New password" rows="8" :state="validateState('newpassword')"
                                    aria-describedby="feedback_3">
                                </b-form-input>
                                <b-form-invalid-feedback id="feedback_3" class="text-sm">This is a required field and
                                    must be at
                                    least 8 characters and not more than 24 characters.</b-form-invalid-feedback>
                            </b-form-group>
                            <b-form-group id="input_group_4" label-for="input_4">
                                <b-form-input id="input_confirmpass" v-model="form.confirmpassword" type="password"
                                    placeholder="Confirm password" rows="8" :state="validateState('confirmpassword')"
                                    aria-describedby="feedback_4">
                                </b-form-input>
                                <b-form-invalid-feedback id="feedback_4" class="text-sm">This is a required field and
                                    must be at
                                    least 8 characters and not more than 24 characters.</b-form-invalid-feedback>
                            </b-form-group>
                        </div>
                        <div class="flex justify-center mt-2" v-show="editPassword">
                            <b-button type="submit" variant="outline-primary">Submit</b-button>
                        </div>
                    </b-form>
                </b-card>
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
            editPenname: false,
            editPassword: false,
            pname: '',
            form: {

                oldpassword: '',
                newpassword: '',
                confirmpassword: ''
            },
            userData: '',
            userRole: '',
            userInfo: '',
            editArticleID: '',
            // url: 'http://localhost:3000'
            url: 'https://backend-final.azurewebsites.net'
        }
    },
    validations: {
        form: {
            // pname: {
            //     required,
            //     minLength: minLength(3),
            //     maxLength: maxLength(20),
            // },
            oldpassword: {
                required,
                minLength: minLength(8),
                maxLength: maxLength(24)
            },
            newpassword: {
                required,
                minLength: minLength(8),
                maxLength: maxLength(24)
            },
            confirmpassword: {
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
            setTimeout(() => {
                swal.fire({
                    title: 'You need to log in first!',
                    // text: 'Do you want to continue',
                    icon: 'warning',
                    confirmButtonText: 'Done',
                    confirmButtonColor: '#007bff'
                })
            }, 1000);
        }
    },
    methods: {
        togglePenname() {
            this.editPenname = !this.editPenname
        },
        togglePassword() {
            this.editPassword = !this.editPassword
        },
        validateState(newpassword) {
            const { $dirty, $error } = this.$v.form[newpassword];
            return $dirty ? !$error : null;
        },

        // ----------------------------------------------------------------
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

        // ----------------------------------------------------------------
        // CONFIRM    
        confirmPenname() {
            // await this.reloadUser();        
            swal.fire({
                title: 'Are you sure?',
                text: "After this, your pen name will be changed.",
                icon: 'warning',
                showCancelButton: true,
                confirmButtonColor: '#007bff',
                cancelButtonColor: '#dc2626',
                confirmButtonText: 'Confirm',
                cancelButtonText: 'Cancel'
            }).then((result) => {
                if (result.isConfirmed) {
                    this.submitPenname()
                }
            })
            console.log('SubmitPenname:')
        },
        confirmPassword() {
            // await this.reloadUser();   
            this.$v.form.$touch();
            if (this.$v.form.$anyError) {
                return;
            }
            swal.fire({
                title: 'Are you sure?',
                text: "After this, your password will be changed.",
                icon: 'warning',
                showCancelButton: true,
                confirmButtonColor: '#007bff',
                cancelButtonColor: '#dc2626',
                confirmButtonText: 'Confirm',
                cancelButtonText: 'Cancel'
            }).then((result) => {
                if (result.isConfirmed) {
                    this.submitPassword()
                }
            })
        },

        // ----------------------------------------------------------------
        // EDIT
        showPenname(userInfo) {
            this.editUserID = userInfo.user_id
            this.form.pname = userInfo.alias
        },
        hidePenname() {
            this.editUserID = ''
            this.form.pname = ''
        },
        hidePassword() {
            this.form.oldpassword = ''
            this.form.newpassword = ''
            this.form.confirmpassword = ''
            this.$nextTick(() => {
                this.$v.$reset();
            });
        },
        async submitPenname() {
            console.log('SubmitPenname2:')

            try {
                console.log('NewPenname:')
                console.log(this.form.pname)
                const res = await fetch(this.url + "/editalias", {
                    method: 'PUT',
                    headers: {
                        'Content-type': 'application/json'
                    },
                    credentials: 'include',
                    body: JSON.stringify({
                        // article_id: this.editArticleID,
                        alias: this.pname
                    }),
                })
                const resdata = await res.json()
                if (resdata.data == 1) {
                    console.log('SubmitPenname3:')
                    console.log(resdata)
                    swal.fire({
                        title: 'Change pen name Success!',
                        // text: 'Do you want to continue',
                        icon: 'success',
                        confirmButtonText: 'Done',
                        confirmButtonColor: '#007bff'
                    })
                    const reuser = await fetch(this.url + "/getsingleuser", {
                        headers: {
                            'Content-type': 'application/json'
                        },
                        credentials: 'include'
                    })
                    const getuserdata = await reuser.json()
                    this.userData = getuserdata.data
                    console.log('ReUser:')
                    console.log(this.userData)
                } else if (resdata.data == 0) {
                    swal.fire({
                        title: 'This pen name already exists!',
                        // text: 'This name already exist',
                        icon: 'error',
                        confirmButtonText: 'Done',
                        confirmButtonColor: '#007bff'
                    })
                }

            }
            catch (error) {
                console.log(`EditPenname False!!! ${error}`)
                swal.fire({
                    title: 'Change pen name Error!',
                    // text: '${error}',
                    icon: 'error',
                    confirmButtonText: 'Cancel',
                    confirmButtonColor: '#007bff'
                })
            }
            this.pname = ''
            this.$nextTick(() => {
                this.$v.$reset();
            });

        },

        async submitPassword() {
            this.$v.form.$touch();
            if (this.$v.form.$anyError) {
                return;
            }
            try {
                console.log('Newpassword:')
                console.log(this.form.newpassword)
                console.log('Confirmpassword:')
                console.log(this.form.confirmpassword)
                if (this.form.newpassword === this.form.confirmpassword) {
                    const res = await fetch(this.url + "/editpassword", {
                        method: 'PUT',
                        headers: {
                            'Content-type': 'application/json'
                        },
                        credentials: 'include',
                        body: JSON.stringify({
                            // article_id: this.editArticleID,
                            password: this.form.oldpassword,
                            new_password: this.form.newpassword,
                        })

                    })

                    console.log('Changepassword!')
                    const resdata = await res.json()
                    console.log('respassword')
                    console.log(resdata)
                    if (resdata.data == 1) {
                        swal.fire({
                            title: 'Change password success!',
                            // text: 'Do you want to continue',
                            icon: 'success',
                            confirmButtonText: 'Done',
                            confirmButtonColor: '#007bff'
                        })
                    } else if (this.form.newpassword !== this.form.confirmpassword) {
                        swal.fire({
                            title: 'Your confirm password is wrong!',
                            // text: '${error}',
                            icon: 'error',
                            confirmButtonText: 'Cancel',
                            confirmButtonColor: '#007bff'
                        })

                    }
                    console.log('Changepassword2')
                    // const res = await fetch(this.url + "/getarticleowner", {
                    //     headers: {
                    //         'Content-type': 'application/json'
                    //     },
                    //     credentials: 'include'
                    // })
                    // const getuserdata = await res.json()
                    // this.articles = getuserdata
                    // console.log('Reloaddata:')
                    // console.log(this.articles)

                    // setTimeout(() => { this.$router.go(-1) }, 2000);
                    // console.log('EditForm:')
                    // console.log(this.form.title, this.editArticleID)
                    // this.editArticleID = ''

                }

            }
            catch (error) {
                console.log(`EditPassword False!!! ${error}`)
                swal.fire({
                    title: 'Submit Error!',
                    // text: '${error}',
                    icon: 'error',
                    confirmButtonText: 'Cancel',
                    confirmButtonColor: '#007bff'
                })
            }
            this.form.oldpassword = ''
            this.form.newpassword = ''
            this.form.confirmpassword = ''
            this.$nextTick(() => {
                this.$v.$reset();
            });
        }
    }

}


</script>