<template>
<div class="bg-zinc-800 min-h-screen text-white">
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
export default {
     components: {

      },
    data() {
        return {
            members: [
                { title: "Kamolwish Woramethaleot", sid: "62130500003", position: "Front-End, Database", image: "https://files.catbox.moe/6gtvug.jpg", id: 1 },
                { title: "Korawit Rittisit", sid: "62130500006", position: "DevOps, Network", image: "https://files.catbox.moe/fl67os.jpg", id: 2 },
                { title: "Thanadol Chuesai", sid: "62130500030", position: "Back-End", image: "https://files.catbox.moe/pk3vz2.jpg", id: 3 }
            ]
        }
    }

}
</script>