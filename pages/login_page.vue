<template>
    <div class="bg-zinc-800 min-h-screen text-white">
        <NavBar />
        <b-container fluid>
            <div class="pt-20">
                <loginForm @login-user="login"></loginForm>
            </div>
        </b-container>
    </div>
</template>

<script>
import swal from 'sweetalert2/dist/sweetalert2.js'
import NavBar from '@/components/nav_bar.vue'
import loginForm from '@/components/login_form.vue'

export default {
    name: 'LoginPage',
    components: {
        NavBar,
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

                const resdata = await res.json()
                if (resdata.data == 1) {
                    swal.fire({
                        title: 'Login Success!',
                        // text: 'Your has been registered.',
                        icon: 'success',
                        showConfirmButton: false,
                        timer: 2000
                        // confirmButtonColor: '#007bff',
                        // confirmButtonText: 'Done',                    
                    })
                    setTimeout(() => { this.$router.push({ name: 'index' }) }, 1000);
                    setTimeout(() => { this.$router.go(0) }, 2000);
                } else if (resdata.data == 0) {
                    swal.fire({
                        title: 'Something went wrong!',
                        text: 'Your Email or Password has been wrong.',
                        icon: 'error',
                        confirmButtonColor: '#007bff',
                        confirmButtonText: 'Done',
                    })
                }
            }
            catch (error) {
                console.log(`LoginFalse!!! ${error}`)
                swal.fire({
                    title: 'Login Failed!',
                    // text: 'Your has been registered.',
                    icon: 'error',
                    confirmButtonColor: '#007bff',
                    confirmButtonText: 'Done',
                })
            }
        },
    }
}


</script>