<template>
    
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
    
</template>

<script>
export default {
    data() {
        return {
            members: [
                { title: "Kamolwish Woramethaleot", sid: "62130500003", position: "Front-End, Database", image: "https://scontent.fbkk5-6.fna.fbcdn.net/v/t39.30808-6/257101106_955566288383122_6618791366635269599_n.jpg?_nc_cat=101&ccb=1-5&_nc_sid=09cbfe&_nc_eui2=AeFzgzZSfnYr6Wug6Wv1cfdOtzFY3LjPJLW3MVjcuM8ktaUc_WGIo7hvSuPS0rLBsIenDKj_jV_RamGl8siRYAsQ&_nc_ohc=tE0W7t2XMMUAX9p2-wF&_nc_ht=scontent.fbkk5-6.fna&oh=00_AT8V4lfR-tD4-2xcxpiZc2HkGqvmu0FfN2a2mVE05ly3uQ&oe=62630620", id: 1 },
                { title: "Korawit Rittisit", sid: "62130500006", position: "DevOps, Network", image: "https://picsum.photos/600/300/?image=25", id: 2 },
                { title: "Thanadol Chuesai", sid: "62130500030", position: "Back-End", image: "https://picsum.photos/600/300/?image=25", id: 3 }
            ]
        }
    }

}
</script>