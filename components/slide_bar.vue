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
              <b-nav-item to="show_discuss" @click="hide">Articles</b-nav-item>
              <b-nav-item to="/about" @click="hide">About</b-nav-item>
              <b-nav-item to="/member" @click="hide">Member</b-nav-item>
              <b-nav-item to="/register_page" @click="hide" v-if="userData == null">Register</b-nav-item>              
              <b-nav-item to="/login_page" @click="hide" v-if="userData == null">Login</b-nav-item>
              <b-nav-item to="/manage_page" @click="hide" v-if="userRole == 2">Manage</b-nav-item>                            
              <b-nav-item to="/" @click="logOut" v-if="userData != null">Logout</b-nav-item>                    
              <b-nav-item to="/userInfo_page" v-if="userName != null" class="absolute bottom-0"> {{userName}} </b-nav-item>
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
      auth: false,
      userName: '',
      userData: null,
      userRole: '',
      Role:'',
      url: 'http://localhost:3000'
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
        this.userName = getuserdata.data.name
        console.log('Username:')
        console.log(this.userName)
        // return getuserdata
      }
      catch (error) {
        console.log(`get user failed: ${error}`)
      }
  },
  // mounted() {
  //   this.$nuxt.$on('auth', auth => {
  //     this.auth = auth;
  //   })
  // },
  // async created() {
  //   this.userData = await this.getUser();
  //   console.log('Userdata:')
  //   console.log(this.userData)

    // this.userName = this.userData.data.name

    // this.userRole = await this.getRole();
    // console.log('Userrole:')
    // console.log(this.userRole)

    // this.Role = this.userRole.data.role
    // console.log('role:')
    // console.log(this.Role)
  // },

  methods: {
    // async getUser() {
      
        
    // },

    // async getRole() {
    //   if (document.cookie == null) { return }
    //   try {
    //     const res = await fetch(this.url + "/getsingleuser", {
    //       credentials: 'include'
    //     })
    //     const getuserdata = await res.json()        
    //     return getuserdata
    //   }
    //   catch (error) {
    //     console.log(`get user failed: ${error}`)
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
        // this.$emit('logout')
        // this.$router.go(0);
        const resdata = await res.json()
        console.log('Logout Complete!!:')
        if (resdata.data == 1) {
          swal.fire({
            title: 'Logout Completed!',
            icon: 'success',
            confirmButtonColor: '#007bff',
            confirmButtonText: 'Done',
          })
        }
        // this.$router.go(0)
        setTimeout(() => { this.$router.go(0) }, 2000);
      }
      catch (error) { console.log(`Log Out failed: ${error}`) }
    },
  },
  
}

</script>