<template>

  <div class="bg-zinc-800 min-h-screen text-white">

    <SlideBar class="pt-20 sm:pl-4 md:pl-12 lg:pl-24 xl:pl-48" />
    <div class="justify-center flex">
      <div class=" mt-12 mx-2 p-4 border-solid border-2 border-primary rounded-md h-auto w-60 lg:w-max md:w-max">
        <!-- <div class="grid grid-cols-2 gap-20 justify-center md:grid-cols-2 cols sm:grid-cols-1"> -->

        <p class="text-2xl font-bold">
          POWERED BY:
        </p>
        <div class="flex mt-4">
          <div class="p-2 flex-auto w-40" bg-variant="dark">
            <img src="@/assests/image/TMDB.svg" alt="TMDB" />
          </div>
        </div>

        <div class="flex mt-4">
          <div class="p-2 flex-auto w-40" bg-variant="dark">
            <img src="@/assests/image/colored-white-text.svg" alt="NUXT" />
          </div>

          <div class="p-2 flex-auto w-80" bg-variant="dark">
            <img src="@/assests/image/tailwindcss-logotype-white.svg" alt="Tailwind" />
          </div>
        </div>


        <!-- <b-col col lg="6" md="6" sm="1">
            <b-card class="p-2" bg-variant="dark" text-variant="light" cols="1">
              <b-card-text cols="1">
                A place for movies and people who want to comment on the subject so that people can decide whether or
                not
                to watch the movies.
              </b-card-text>
            </b-card>
          </b-col> -->
        <!-- </b-row> -->
        <!-- </div> -->

      </div>
    </div>

  </div>


</template>

<script>

import SlideBar from '@/components/slide_bar.vue'
export default {
  name: "AboutPage",
  components: {
    SlideBar
  },
  layout: "",

}
</script>


