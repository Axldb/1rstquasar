<template>
    <div id="app">
        <AppHeader />
        <div>
            <AppBody />
        </div>

    </div>
</template>

<script setup lang="ts">

    import AppHeader from './components/FilmHeader.vue';

    import AppBody from './components/FilmBody.vue';
 
</script>
<style>
    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        text-align: -webkit-center;
    }
</style>
