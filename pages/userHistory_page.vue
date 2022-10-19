<template>
    <div class="bg-zinc-800 min-h-screen text-white">
        <b-container>
            <b-row align-h="between">
                <b-col cols="9" xl="10" lg="10" md="10" sm="10">
                    <SlideBar class="ml-2 mt-20 xs:ml-20 sm:ml-20 md:ml-20 lg:ml-20 xl:ml-20" />                    
                </b-col>
                <b-col>
                    
                </b-col>
            </b-row>
        </b-container>

        <div class="pt-20 flex justify-center">
            <b-col col xl="12" lg="12" md="12" sm="12">

                

            </b-col>
        </div>

    </div>
</template>

<script>
import SlideBar from '@/components/slide_bar.vue'

export default {
    name: 'userHistoryPage',
    components: {
        SlideBar
    },
    data() {
        return {
            url: 'http://localhost:3000'
            // url: 'https://backend-final.azurewebsites.net'
        }
    },
    methods: {

    }
}
</script>