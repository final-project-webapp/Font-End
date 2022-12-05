<template>
    <div class="text-white">
        <b-container class="justify-center flex">
            <div style="max-width: 800px; max-height: 600px;"
                class="mt-40 mx-2 p-4 border-solid border-2 border-primary rounded-md">

                <div class="flex justify-center mt-4">                
                    <b-icon icon="three-dots" animation="cylon" font-scale="7"></b-icon>                
                </div>

                <div class="flex justify-center mt-4">
                    <p class="text-4xl font-bold">
                        Loading your request article...
                    </p>
                </div>                
            </div>
        </b-container>
    </div>
</template>

<script>
export default {
    name: 'FailedArticlePage',
}
</script>