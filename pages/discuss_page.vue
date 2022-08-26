<template>
    <div class="bg-zinc-800 min-h-screen text-white">
        <b-container fluid>
            <b-row align-h="between">
                <b-col cols="4">
                    <SlideBar class="ml-2 pt-20 lg:pl-20 xl:pl-44" />
                </b-col>
                <b-col cols="2">

                </b-col>
            </b-row>
        </b-container>

        <DF />

    </div>
</template>

<script>
import SlideBar from '@/components/slide_bar.vue'
import DF from '@/components/discuss_form.vue'

export default {
    name: 'DiscussPage',
    components: {
        SlideBar,
        DF

    },
    data() {
        return {
            
        }
    },
    
}


</script>