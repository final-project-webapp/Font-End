<template>
  <div class="bg-zinc-800 min-h-screen text-white">
    <!-- <SlideBar class="pt-20 sm:pl-4 md:pl-12 lg:pl-24 xl:pl-48" /> -->
    <NavBar class="pt-20" />
    <div class="justify-center flex">
      <div class=" mt-12 mx-2 p-4 border-solid border-2 border-primary rounded-md h-auto w-60 lg:w-max md:w-max">
        <!-- <div class="grid grid-cols-2 gap-20 justify-center md:grid-cols-2 cols sm:grid-cols-1"> -->

        <p class="text-2xl font-bold">
          POWERED BY:
        </p>
        <div class="flex mt-4">
          <div class="p-2 flex-auto w-80" bg-variant="dark">
            <img src="@/assests/image/TMDB.svg" alt="TMDB" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

// import SlideBar from '@/components/slide_bar.vue'
import NavBar from '@/components/nav_bar.vue'
export default {
  name: "AboutPage",
  components: {
    NavBar
  },
  layout: "",

}
</script>


