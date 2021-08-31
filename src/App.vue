<template>
    <div id="app">
        <h3 class="title">TODO list</h3>
        <Tasks />
    </div>
</template>

<script>
import Tasks from "./components/Tasks.vue"

export default {
    name: 'App',
    components: {
        Tasks
    }
}
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
    max-width: 800px;
    margin-left: auto;
    margin-right: auto;
}
.title {
    margin-bottom: 80px;
}
.sr-only {
    display: none;
}
</style>
