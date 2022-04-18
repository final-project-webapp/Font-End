<template >
<div>
<b-button v-b-toggle.my-sidebar>Toggle Sidebar</b-button>
<b-sidebar id="my-sidebar" title="Sidebar" shadow bg-variant="dark" text-variant="light">
     <template #default="{ hide }">
        <div class="p-3">
          <h4>Custom header sidebar</h4>
          <p>
            Cras mattis consectetur purus sit amet fermentum. Cras justo odio, dapibus ac facilisis
            in, egestas eget quam. Morbi leo risus, porta ac consectetur ac, vestibulum at eros.
          </p>
          <nav class="mb-3">
            <b-nav vertical>
              <b-nav-item to="/" @click="hide" >Home</b-nav-item>
              <b-nav-item to="/about" @click="hide">About</b-nav-item>
              <b-nav-item to="/member" @click="hide">Member</b-nav-item>
            </b-nav>
          </nav>        
        </div>
      </template>
    </b-sidebar>
    <nuxt />
</div>
</template>