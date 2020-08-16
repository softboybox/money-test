<template>
    <div id="app">
        <Header/>
        <MainPage class="main-wrapper"/>
    </div>
</template>

<script>
    import MainPage from './components/MainPage.vue'
    import Header from './components/Header.vue'

    export default {
        name: 'App',
        components: {
            MainPage, Header
        }
    }
</script>

<style lang="scss">
    body{
        margin: 0;
        padding: 0;
    }
    .main-wrapper{
        margin: 21px 24px 0;
        @media screen and (min-width: 1320px) {
            margin: 80px 240px 0;
        }
    }
    h1{
        font-style: normal;
        font-weight: 300;
        font-size: 25px;
        line-height: 26px;
        color: #000000;
        text-align: center;
        margin-bottom: 33px;
    }
</style>
