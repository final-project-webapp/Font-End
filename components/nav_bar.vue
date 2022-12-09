<template >
    <div>
        <b-navbar toggleable="lg" type="dark" variant="dark" fixed="top">
            <b-navbar-brand href="#" to="/">MediaRE</b-navbar-brand>

            <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

            <b-collapse id="nav-collapse" is-nav>
                <b-navbar-nav type="dark" variant="dark">
                    <b-nav-item href="#" to="/">Now Playing</b-nav-item>
                    <b-nav-item href="#" to="/show_discuss">See Articles</b-nav-item>
                    <b-nav-item-dropdown text="About" right>
                        <b-dropdown-item href="#" to="/about">Powered</b-dropdown-item>
                        <b-dropdown-item href="#" to="/member">Team</b-dropdown-item>
                    </b-nav-item-dropdown>
                </b-navbar-nav>

                <!-- Right aligned nav items -->
                <b-navbar-nav class="ml-auto">
                    <b-nav-item href="#" to="/register_page" v-if="userData == null">Register</b-nav-item>
                    <b-nav-item href="#" to="/login_page" v-if="userData == null">Log in</b-nav-item>
                    
                    <b-nav-item-dropdown right v-if="userData != null">
                        <!-- Using 'button-content' slot -->
                        <template #button-content>
                            <em> {{ userName }} </em>
                        </template>
                        <b-dropdown-item href="#" to="/userInfo_page" v-if="userRole == 1">Profile</b-dropdown-item>
                        <b-dropdown-item href="#" to="/manage_page" v-if="userRole == 2">Manage</b-dropdown-item>
                        <b-dropdown-item href="#" to="/" @click="logOut()" v-if="userData != null">Log Out</b-dropdown-item>
                    </b-nav-item-dropdown>
                </b-navbar-nav>
            </b-collapse>
        </b-navbar>
    </div>
</template>
  
<script>
import swal from 'sweetalert2/dist/sweetalert2.js'
export default {
    name: 'NavBar',
    data() {
        return {
            userName: '',
            userData: null,
            userRole: null,
            url: 'http://localhost:3000'
            // url: 'https://backend-final.azurewebsites.net'
        }
    },

    async mounted() {
        console.log('UserData_Sliebar')
        console.log(this.userData)
        if (document.cookie == null) { return }
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

            console.log(this.userName)
            console.log("getUser Center")
        }
        catch (error) {
            console.log(`get user failed: ${error}`)
        }
    },
    methods: {
        // GET
        // async getCookie() {
        //   try {
        //     await fetch(this.url + "/getcookie", {
        //       headers: {
        //         'Content-type': 'application/json'
        //       },
        //       credentials: 'include'
        //     })

        //   }
        //   catch (error) {
        //     console.log(`get cookie failed: ${error}`)
        //   }
        // },

        async logOut() {
            try {
                const res = await fetch(this.url + "/logout", {
                    method: 'POST',
                    headers: {
                        'Content-type': 'application/json'
                    },
                    credentials: 'include'
                })
                const resdata = await res.json()
                console.log('Logout Complete!!:')
                console.log(this.userData)
                if (resdata.data == 1) {
                    swal.fire({
                        title: 'Logout completed!',
                        icon: 'success',
                        // confirmButtonColor: '#007bff',
                        // confirmButtonText: 'Done',
                        showConfirmButton: false,
                        timer: 2000
                    })
                    setTimeout(() => { this.$router.push({ name: 'index' }) }, 1000);
                    setTimeout(() => { this.$router.go(0) }, 2000);
                }

            }
            catch (error) {
                console.log(`Log Out failed: ${error}`)
                swal.fire({
                    title: 'Logout Failed!',
                    // text: 'Your has been registered.',
                    icon: 'error',
                    confirmButtonColor: '#007bff',
                    confirmButtonText: 'Done',
                })
            }
        },
    },

}

</script>