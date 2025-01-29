<template>
    <div>
        <div class="spinner-border text-success" role="status">
            <span class="visually-hidden">Loading...</span>
        </div> {{ title }}...
    </div>
</template>

<script>
    export default {
        name:'Loading',
        props:{
            title: String
        }
    }
</script>

<style>

</style>