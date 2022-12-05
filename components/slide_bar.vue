<template >
  <div>
    <b-button v-b-toggle.my-sidebar variant="">
      <b-icon icon="list" variant="primary" font-scale="2" animation=""></b-icon>
    </b-button>
    <b-sidebar id="my-sidebar" title="Menu" shadow bg-variant="dark" text-variant="light">
      <template #default="{ hide }">
        <div class="p-3">
          <nav class="mb-3">
            <b-nav vertical>
              <b-nav-item to="/" @click="hide">Now Playing</b-nav-item>
              <b-nav-item to="/show_discuss" @click="hide">Articles</b-nav-item>
              <b-nav-item to="/about" @click="hide">About</b-nav-item>
              <b-nav-item to="/member" @click="hide">Team</b-nav-item>
              <b-nav-item to="/register_page" @click="hide" v-if="userData == null">Register</b-nav-item>
              <b-nav-item to="/login_page" @click="hide" v-if="userData == null">Login</b-nav-item>
              <b-nav-item to="/manage_page" @click="hide" v-if="userRole == 2">Manage</b-nav-item>
              <b-nav-item to="/" @click="logOut" v-if="userData != null">Logout</b-nav-item>
              <b-nav-item to="/userInfo_page" v-if="userRole == 1" class="absolute bottom-0"> {{ userName }}
              </b-nav-item>
              <!-- <b-nav-item @click="getCookie()"> GetCookie -->
              <!-- </b-nav-item> -->
              <b-nav-text v-if="userRole == 2" class="absolute bottom-0 left-8 texl-xl font-bold"> Admin
              </b-nav-text>
            </b-nav>
          </nav>
        </div>
      </template>
    </b-sidebar>
    <nuxt />
  </div>
</template>

<script>
import swal from 'sweetalert2/dist/sweetalert2.js'
export default {
  name: 'SlideBar',
  data() {
    return {
      userName: '',
      userData: null,
      userRole: null,
      // url: 'http://localhost:3000'
      url: 'https://backend-final.azurewebsites.net'
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