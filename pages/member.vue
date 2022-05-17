<template>
  <div class="bg-zinc-800 min-h-screen">
    <SlideBar class="pl-20 pt-10"/>

    <MemberList class="pt-20" />
  </div>
</template>

<script>
import SlideBar from '@/components/slide-bar.vue'
import MemberList from '@/components/MemberList.vue'
export default {
  name: 'MemberPage',
  components: {
    MemberList,
    SlideBar
  },
  layout: "nav-bar"
}
</script>
