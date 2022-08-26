<template>
  <div class="bg-zinc-800 min-h-screen">
    <SlideBar class="ml-2 pt-20 lg:pl-20 xl:pl-44"/>

    <MemberList class="pt-20" />
  </div>
</template>

<script>
import SlideBar from '@/components/slide_bar.vue'
import MemberList from '@/components/MemberList.vue'
export default {
  name: 'MemberPage',
  components: {
    MemberList,
    SlideBar
  },
  layout: "nav_bar"
}
</script>
