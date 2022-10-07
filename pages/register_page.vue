<template>
    <div class="bg-zinc-800 min-h-screen text-white">
        <b-container fluid>
            <b-row align-h="between">
                <b-col cols="9" xl="10" lg="10" md="10" sm="10">
                    <SlideBar class="pt-20 sm:pl-4 md:pl-12 lg:pl-24 xl:pl-48" />
                </b-col>
                <b-col cols="2">

                </b-col>
            </b-row>
        </b-container>

        <registerForm @register-user="confirmRegis"></registerForm>

    </div>
</template>

<script>
import swal from 'sweetalert2/dist/sweetalert2.js'
import SlideBar from '@/components/slide_bar.vue'
import registerForm from '@/components/register_form.vue'


export default {

    name: 'RegisterPage',
    components: {
        SlideBar,
        registerForm
    },
    emits: ['register-user'],
    data() {
        return {
            userList: [],
            url: 'http://localhost:3000'
        }
    },
    async created() {
        this.userList = await this.getAllUser();
        console.log('userList')
        console.log(this.userList)
    },

    methods: {
        // CONFIRM
        async confirmRegis(registerData) {
            await this.reloadUser();
            console.log('registerData_page')
            console.log(registerData)
            swal.fire({
                title: 'Are you sure?',
                text: "You won't be able to revert this!",
                icon: 'warning',
                showCancelButton: true,
                confirmButtonColor: '#007bff',
                cancelButtonColor: '#dc2626',
                confirmButtonText: 'Confirm',
                cancelButtonText: 'Cancel'
            }).then((result) => {
                if (result.isConfirmed) {
                    this.registerUser(registerData)                                        
                }
            })
        },

        // GET
        async getAllUser() {
            try {
                const res = await fetch(this.url + "/getalluser", {
                    credentials: 'include'
                })
                const getuserdata = await res.json()
                return getuserdata
            }
            catch (error) {
                console.log(`get user failed: ${error}`)
            }
        },

        async reloadUser() {
            this.userList = await this.getAllUser()
        },



        // POST
        async registerUser(registerData) {
            console.log('regisdata2')
            console.log(registerData)
            try {
                const res = await fetch(this.url + "/adduser", {
                    method: 'POST',
                    headers: {
                        'Content-type': 'application/json'
                    },
                    body: JSON.stringify({
                        name: registerData.userName,
                        emailaddress: registerData.emailAddress,
                        password: registerData.password,
                        DOB: registerData.dob
                    })
                })
        
                const resdata = await res.json()
                if (resdata.data == 1) {
                    swal.fire({
                        title: 'Registered!',
                        text: 'Your has been registered.',
                        icon: 'success'
                    })
                    setTimeout(() => { this.$router.push('/login_page') }, 2000);
                } else {
                    swal.fire({
                        title: 'Register Failed!',
                        text: 'Your email has been used.',
                        icon: 'error'
                    })
                }                              
            }
            catch (error) {
                console.log(`addUserFalse!!! ${error}`)
            }            
        }
    },


}


</script>