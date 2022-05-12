<template>
<div class="bg-zinc-800 min-h-screen text-white">
    <SlideBar class="pl-20 pt-10"/>
        <b-container fluid>
        <b-row align-h="center">
            <div v-for="member in members" :key="member.id">                
                <b-col lg="">
                    <b-card
                        :title="member.title"
                        :img-src="member.image"
                        img-alt="Image"
                        img-top
                        tag="article"
                        style="max-width: 20rem;"
                        class="mb-2"
                        bg-variant="dark"
                        text-variant="light"
                    >
                        <b-card-text>{{ member.sid }}</b-card-text>
                        <b-card-text>{{ member.position }}</b-card-text>                        
                    </b-card>
                </b-col>
            </div>
        </b-row>
        </b-container>
    </div>
</template>

<script>
import SlideBar from '@/components/slide-bar.vue'
export default {
     components: {
      SlideBar
      },
    data() {
        return {
            members: [
                { title: "Kamolwish Woramethaleot", sid: "62130500003", position: "Front-End, Database", image: "https://files.catbox.moe/lz6xvn.jpeg", id: 1 },
                { title: "Korawit Rittisit", sid: "62130500006", position: "DevOps, Network", image: "https://picsum.photos/600/300/?image=25", id: 2 },
                { title: "Thanadol Chuesai", sid: "62130500030", position: "Back-End", image: "https://picsum.photos/600/300/?image=25", id: 3 }
            ]
        }
    }

}
</script>