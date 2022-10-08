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
              <b-nav-item to="/register_page" @click="hide">Register</b-nav-item>
              <b-nav-item to="/login_page" @click="hide">Login</b-nav-item>
              <b-nav-item>
                <b-button @click="logOut">Logout</b-button>
              </b-nav-item>
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
      url: 'http://localhost:3000'
    }
  },
  methods: {
    async logOut() {
      console.log('Logout!!:')
      try {
        await fetch(this.url + "/logout", {
          method: 'POST',
          headers: {
            'Content-type': 'application/json'
          },
          credentials: 'include',
        })
        // this.$emit('logout')
        // this.$router.go(0);
        console.log('Logout Complete!!:')
        swal.fire({
          title: 'Logout Completed!',          
          icon: 'success',
          confirmButtonColor: '#007bff',
          confirmButtonText: 'Done',
        })
      }
      catch (error) { console.log(`Log Out failed: ${error}`) }
    },
  }

}

</script>