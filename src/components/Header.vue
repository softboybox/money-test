<template>
    <div class="header">
        <div class="header__title">
            <a href="#"><img alt="logo" src="image/logo.svg"></a>
        </div>
        <div class="header__switcher">
            <div class="header__switcher_active">РУ</div>
            <span></span>
            <div>УКР</div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Header"
    }
</script>

<style lang="scss">
    .header{
        background: linear-gradient(90deg, #5A4BE6 0.26%, #73AFF7 47.1%, #93D0D9 99.8%);
        width: 100%;
        max-height: 64px;
        padding: 22px 24px;
        box-sizing: border-box;
        display: flex;
        justify-content: space-between;
        align-items: center;
        @media screen and (min-width: 1320px) {
            padding: 40px 220px;
        }
        &__switcher{
            display: flex;
            font-style: normal;
            font-weight: normal;
            font-size: 16px;
            line-height: 19px;
            letter-spacing: -0.2px;
            color: white;
            align-items: center;
            &_active{
                color: #587BC3;
            }
            div{
                margin: 0 2px;
                cursor: pointer;
            }
            span{
                display: block;
                background: white;
                transform: rotate(-90deg);
                width: 15px;
                height: 1px;
            }
        }
        &__title{
            @media screen and (min-width: 1320px) {
                width: 100%;
                text-align: center;
            }
        }
    }
</style>