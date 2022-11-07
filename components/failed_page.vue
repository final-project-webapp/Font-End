<template>
    <div class="text-white">
        <b-container class="justify-center flex">
            <div style="max-width: 800px; max-height: 600px;"
                class="mt-40 mx-2 p-4 border-solid border-2 border-primary rounded-md">

                <div class="flex justify-center mt-4">
                    <b-icon icon="x-circle-fill" variant="danger" font-scale="7">
                    </b-icon>
                </div>

                <div class="flex justify-center mt-4">
                    <p class="text-4xl font-bold">
                        You don't have privilege here.
                    </p>
                </div>
                <!-- <div class="flex justify-center mt-4">
                    <p class="">
                        Please Contact Your Admin
                    </p>
                </div> -->

                <!-- <div class="flex justify-center mt-20">
                    <b-button variant="primary">
                        <NuxtLink class="" :to="{ name: 'index' }">
                            Back to Homepage
                        </NuxtLink>
                    </b-button>
                </div> -->

            </div>
        </b-container>
    </div>
</template>

<script>
export default {
    name: 'FailedPage',
}
</script>