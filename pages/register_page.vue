<template>
    <div class="bg-zinc-800 min-h-screen text-white">
        <b-container fluid>
            <b-row align-h="between">
                <b-col cols="9" xl="10" lg="10" md="10" sm="10">
                    <SlideBar class="pt-20 sm:pl-4 md:pl-12 lg:pl-24 xl:pl-48" />
                </b-col>
                <b-col cols="2">

                </b-col>
            </b-row>
        </b-container>

        <RF />

    </div>
</template>

<script>
import SlideBar from '@/components/slide_bar.vue'
import RF from '@/components/register_form.vue'

export default {
    name: 'RegisterPage',
    components: {
        SlideBar,
        RF

    },
    data() {
        return {
            
        }
    },
    
}


</script>