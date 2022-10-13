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

        <loginForm @login-user="login"></loginForm>

    </div>
</template>

<script>
import swal from 'sweetalert2/dist/sweetalert2.js'
import SlideBar from '@/components/slide_bar.vue'
import loginForm from '@/components/login_form.vue'

export default {
    name: 'LoginPage',
    components: {
        SlideBar,
        loginForm

    },
    emits: ['login-user'],
    data() {
        return {
            // url: 'http://localhost:3000'
            url: 'https://backend-final.azurewebsites.net'
        }
    },
    methods: {
        async login(loginData) {
            console.log('logindata_page')
            console.log(loginData)
            try {
                const res = await fetch(this.url + "/login", {
                    method: 'POST',
                    headers: {
                        'Content-type': 'application/json'
                    },
                    credentials: 'include',
                    body: JSON.stringify({
                        emailaddress: loginData.emailAddress,
                        password: loginData.password,
                    })
                })
                console.log('Done!!')
                // console.log(loginData)

                const resdata = await res.json()
                if (resdata.data == 1) {
                    swal.fire({
                        title: 'Login Success!',
                        // text: 'Your has been registered.',
                        icon: 'success',
                        confirmButtonColor: '#007bff',
                        confirmButtonText: 'Done',
                    // }).then((result) => {
                    //     if (result.isConfirmed) {
                            
                    //     }
                    })
                }
                setTimeout(() => { this.$router.push('/') }, 2000);
                // this.$router.go(0)                
            }
            catch (error) {
                console.log(`LoginFalse!!! ${error}`)
            }
        },
    }
}


</script>