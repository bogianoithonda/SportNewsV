<template>
    <div>
        <news></news>
        <!-- <fixture></fixture>
        <group></group> -->
    </div>
</template>

<script>
    import News from '@/components/News.vue'
    import Fixture from '@/components/Fixture.vue'
    import Group from '@/components/Group.vue'
    export default {
        components: {News, Fixture, Group},
        name: 'App',
    }
</script>

<style>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }
</style>
