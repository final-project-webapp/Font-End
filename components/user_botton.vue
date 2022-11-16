<template>
    <div>
        <b-button-group>
            <b-button variant="info">
                <NuxtLink class="" :to="{ name: 'userInfo_page' }">
                    Info
                </NuxtLink>
            </b-button>
            <b-button variant="primary">
                <NuxtLink class="" :to="{ name: 'userData_page' }">
                    History
                </NuxtLink>
            </b-button>
        </b-button-group>
    </div>
</template>

<script>
export default {
  name: 'UserBotton'
}
</script>