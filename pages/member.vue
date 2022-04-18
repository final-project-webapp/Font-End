<template>
  <div class="bg-zinc-800 min-h-screen">
    

    <MemberList class="pt-20" />
  </div>
</template>

<script>

import MemberList from '@/components/MemberList.vue'
export default {
  name: 'MemberPage',
  components: {
    MemberList
  },
  layout: "nav"
}
</script>
