<template>
<div>
    <b-button id="own-discuss" v-b-tooltip.hover.top="'Write your own discuss'" variant="outline-primary">
        <nuxt-link class="" to="/discuss_page">
            <b-icon icon="pencil-fill" animation="fade" variant="" font-scale="1"></b-icon>  
        </nuxt-link>
    </b-button>

    <!-- <b-tooltip target="own-discuss" triggers="hover" placement="top">
    Start you own discuss
    </b-tooltip> -->
</div>
</template>