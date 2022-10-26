<template>
    <div class="bg-zinc-800 min-h-screen text-white">
        <b-container class="justify-center flex">
            <div style="width: 800px; height: 600px;"
                class="mt-20 mx-2 p-4 border-solid border-2 border-primary rounded-md">

                <div class="flex justify-center mt-4">
                    <b-icon icon="door-open-fill" variant="light" font-scale="7">
                    </b-icon>
                </div>

                <div class="flex justify-center mt-4">
                    <p class="text-2xl font-bold">
                        Something Went Wrong
                    </p>
                </div>
                <div class="flex justify-center mt-4">
                    <p class="">
                        Please Contact Your Admin
                    </p>
                </div>

                <div class="flex justify-center mt-20">
                    <b-button variant="primary">
                        <NuxtLink class="" :to="{ name: 'index' }">
                            Back to Homepage
                        </NuxtLink>
                    </b-button>
                </div>

            </div>
        </b-container>
    </div>
</template>

<script>
export default {
    name: 'ErrorPage',
}
</script>