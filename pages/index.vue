<template>
 <div class="bg-zinc-800 min-h-screen text-white">
   
   <Header />
   
 </div>
</template>

<script>
import Header from '@/components/Hea-der.vue'
export default {
  name: 'IndexPage',
  components: {
    Header
  },
  layout:"nav"
}
</script>
