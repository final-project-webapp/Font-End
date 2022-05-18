<template>

  <div class="bg-zinc-800 min-h-screen text-white">

    <SlideBar class="ml-2 pt-20 lg:pl-20 xl:pl-44" />
<div class="justify-center flex">
    <div class=" mt-20 mx-2 p-2 border-solid border-2 border-primary rounded-md h-auto w-60 lg:w-max md:w-max
               ">
      <!-- <div class="grid grid-cols-2 gap-20 justify-center md:grid-cols-2 cols sm:grid-cols-1"> -->

        <b-row align-v="center" class="">
          <b-col col lg="6" md="6" sm="1">
            <b-card img-src="@/assests/image/TMDB.svg" img-alt="TMDB" img-bottom style="max-width: 20rem;" class="p-2"
              bg-variant="dark" text-variant="light">
              <a href="https://www.themoviedb.org"></a>
              <b-card-text>
                POWERED BY:
              </b-card-text>
            </b-card>
            </b-col>
            
            <b-col col lg="6" md="6" sm="1">
            <b-card class="p-2" bg-variant="dark" text-variant="light" cols="1">
              <b-card-text cols="1">
                jddkalfkdlkfjakjfjjfjfjfsknfkansfnl;gnldfkgnlfnglngnglkngl
              </b-card-text>
            </b-card>
          </b-col>
        </b-row>

      <!-- </div> -->
    </div>
    </div>




  </div>


</template>

<script>

import SlideBar from '@/components/slide-bar.vue'
export default {
  name: "AboutPage",
  components: {
    SlideBar
  },
  layout: "",

}
</script>


