<script setup>
import {ref} from "vue";
import Lightbox from "@/components/Lightbox.vue";
import {useAFKStore} from "@/stores/afk.js";
import Vote from "@/components/vote.vue";
import Icon from "@/components/icon.vue";

const is_lightbox = ref(false);
const active = ref(false);


// const image_url = new URL(`@/assets/images/screens/main.png`, import.meta.url);
const russia_logo = new URL(`@/assets/icons/russia-logo.svg`, import.meta.url);
const game_icon = new URL(`@/assets/icons/game-logo-btn.svg`, import.meta.url);


function showLightbox() {
    is_lightbox.value = true;
}

function closeLightbox() {
    is_lightbox.value = false;
}

function iframeLoad() {
    console.log("IFrame Loaded");
}



const afk_store = useAFKStore();
afk_store.is_afk_watcher_enabled = false;
</script>

<template>
    <main class="main-page page">
        <div class="main-page__content">
            <div class="content__left-side">
                <img :src="russia_logo" alt="" class="logotype__image">
                <vote class="qr-code__image" :isActive="false"
                />
                <div
                    class="left-side__button desctop"
                    @mouseover="active = true"
                    @mouseleave="active = false"
                    @click="showLightbox"
                      >
                    <icon :name="'button'"/>
                    <div class="left-side__text">
                        <p>Голосуй за</p>
                        <p style="font-size: calc(90 / 1920 * 100vw);">ОСЕТИЮ</p>
                    </div>
                </div>
            </div>
            <div class="content__right-side">
                <router-link class="button button-bottom mobile" to="/start">
                        <div class="button__text">
                            <p>Экшн-игра «НАРТИАДА»</p>
                        </div>
                    </router-link>
                <router-link class="button button-top" to="/start">
                    <div class="button__text">
                        <p>Викторина АЛАНКА</p>
                    </div>
                </router-link>
                <div class="links">
                <div class="link-wrapper">
                    <router-link class="item-button item-button--1 " to="/videos/1">
                        <icon class="item-button__bg item-button__bg--1" :name="'spiral'"/>
                        <div class="item-button__text1">
                            <p>Спорт</p>
                        </div>
                    </router-link>
                </div>
                <div class="link-wrapper">
                    <router-link class="item-button item-button--2" to="/videos/2">
                        <icon class="item-button__bg item-button__bg--2" :name="'spiral'"/>
                        <div class="item-button__text2">
                            <p style="">Вайб</p>
                            <p style="">ВЛАДИКАВКАЗА</p>
                        </div>
                    </router-link>
                </div>
                <div class="link-wrapper">
                    <router-link class="item-button item-button--3" to="/videos/3">
                        <icon class="item-button__bg item-button__bg--3" :name="'spiral'"/>
                        <div class="item-button__text3">
                            <p style="">Для</p>
                            <p style="">молодёжи</p>
                        </div>
                    </router-link>
                </div>
                <div class="link-wrapper">
                    <router-link class="item-button item-button--4" to="/videos/4">
                        <icon class="item-button__bg item-button__bg--4" :name="'spiral'"/>
                        <div class="item-button__text4">
                            <p style="">КУЛЬТУРНАЯ</p>
                            <p style="">жизнь</p>
                        </div>
                    </router-link>
                </div>
                <div class="link-wrapper">
                    <router-link class="item-button item-button--5" to="/videos/5">
                        <icon class="item-button__bg item-button__bg--5" :name="'spiral'"/>
                        <div class="item-button__text5">
                            <p style="">ПУТЕШЕСТВИЯ</p>
                            <p style="">в Осетию</p>
                        </div>
                    </router-link>
                </div>
                <div class="link-wrapper">
                    <router-link class="item-button item-button--6" to="/videos/6">
                        <icon class="item-button__bg item-button__bg--6" :name="'spiral'"/>
                        <div class="item-button__text6">
                            <p style="">РОСТ</p>
                            <p style="">производства</p>
                        </div>
                    </router-link>
                </div>
            </div>
                <router-link class="button button-bottom desctop" to="/start">
                    <div class="button__text">
                        <p>Экшн-игра «НАРТИАДА»</p>
                    </div>
                </router-link>
                <div
                    class="left-side__button mobile"
                    @mouseover="active = true"
                    @mouseleave="active = false"
                    @click="showLightbox"
                      >
                    <icon :name="'button'"/>
                    <div class="left-side__text">
                        <p>Голосуй за</p>
                        <p style="font-size: calc(90 / 768 * 100vw);">ОСЕТИЮ</p>
                    </div>
                </div>
        </div>
        </div>
            <Lightbox class="lightbox" @close="closeLightbox" :class="{dn: !is_lightbox}">
                <iframe @load="iframeLoad" class="lightbox__iframe"
                        src="http://invmap.web-robot.ru/"></iframe>
            </Lightbox>
    </main>
</template>


<style lang="scss" scoped>


.lightbox.dn{
    visibility: hidden;
}
.main-page {
    height: 100%;
    display: flex;
    align-items: stretch;
    justify-content: flex-start;
    // flex-direction: column;
    position: relative;
    // padding: 0 calc(93 / 1920 * 100vw) calc(63 / 1080 * 100vh);

    &__background {
        position: fixed;
        top: 0;
        left: 0;
        width: 101vw;
        height: 101vh;
        object-fit: cover;
        display: block;
        z-index: -1;
    }

    &__content {
        display: flex;
        // align-items: center;
        justify-content: space-between;
        width: 100%;
        // flex-direction: column;
    }
}

.logotype__image {
        position: absolute;
        top: calc(-70 / 1080 * 100vh);
        height: calc(200 / 1920 * 100vw);
        width: calc(275 / 1920 * 100vw);
        margin-bottom: calc(27 / 1080 * 100vh);
}

.qr-code__image {
        height: calc(520 / 1920 * 100vw);
        width: calc(750 / 1920 * 100vw);
        // margin-bottom: calc(27 / 1080 * 100vh);
}

.content__left-side {
    display: flex;
    flex-direction: column;
    justify-content: space-evenly;
}

.left-side__button {
    position: relative;
    display: flex;
    align-items: center;
    align-self: center;
    justify-content: center;
    width: calc(573 / 1920 * 100vw);
    height: calc(290 / 1920 * 100vw);
    cursor: pointer;
    &:hover {
        .left-side__text {
            color: #FFFFCC;
        }
    }
}

.left-side__text {
    position: absolute;
    font-family: 'Rumpelstiltskin', sans-serif;
    font-weight: 500;
    font-size: calc(60 / 1920 * 100vw);
    line-height: 130%;
    text-align: center;
    color:  white;
    transition:  0.3s ease-in;
    transform: rotate(11deg);
}

.content__right-side {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
}

.button {
    position: absolute;
    background-color: rgba(255, 221, 4, 0.48);
    width: calc(1000 / 1920 * 100vw);
    height: calc(130 / 1920 * 100vw);
    display: flex;
    align-items: center;
    text-decoration: none;
    transition:  0.3s ease-in;
    cursor: pointer;

    &:hover {
        background-color: rgba(255, 221, 4, 1);

        .button__text {
            color: #e92429 !important;

        }
    }
}

.button-top {
    top: calc(-60 / 1080 * 100vh);
    right: -200px;
    padding: 50px 0 34px 150px;
    border-bottom-left-radius: 300px;
}

.button-bottom {
    bottom: 0;
    right: -200px;
    padding: 50px 0px 50px 150px;
    border-top-left-radius: 300px;
}

.button__text {
    font-family: 'Rumpelstiltskin', sans-serif;
    font-weight: 500;
    font-size: calc(46 / 1920 * 100vw);
    line-height: 130%;
    text-align: center;
    color:  #ffdd04 !important;
    opacity: 100%;
    transition:  0.3s ease-in;

    p:last-child {
        font-size: calc(65 / 1920 * 100vw);
        margin-bottom: 0 !important;
    }
}

.links {
    margin: calc(140 / 1080 * 100vh) 0;
    // margin-top: calc(140 / 1080 * 100vh);
    display: grid;
    width: 100%;
    height: 100%;
    grid-template-columns: repeat(3, 1fr);
    justify-content: space-between;
    gap: calc(30 / 1080 * 100vh) 0;
}

.link {
    cursor: pointer;

    &-wrapper {
        display: flex;
        align-items: flex-start;
        margin: 0 auto;

        &:nth-child(2) {
            align-items: center;
        }

        &:nth-child(3) {
            margin-left: 80px;
        }

        &:nth-child(4) {
            position: relative;
            top: -50px;
            right: 50px;
        }

        &:nth-child(5) {
            align-items: center;
        }

        &:nth-child(6) {
            position: relative;
            top: -70px;
        }

        // justify-content: center;
        // width: calc(709 / 1920 * 100vw);

        // &:nth-child(2n) {
        //     margin-left: auto;
        // }
    }

    text-decoration: none;

    outline: none;

    // height: calc(81 / 1080 * 100vh);

    // width: 100%;

}

.item-button {
    font-family: 'Rumpelstiltskin', sans-serif;
    font-size: calc(60 / 1920 * 100vw);
    text-decoration: none !important;
    color: white;
    display: flex;
    align-items: center;
    justify-content: center;
    line-height: 130%;
    text-align: center;
    transition:  0.3s ease-in;

    &:hover{
        // узнать менять ли цвет?
        color: #FFFFCC;

        .item-button__bg {
            opacity: 100;
            animation: 9s linear infinite rotate;
        }
    }

    &--2, &--3,  &--4, &--6 {
        position: relative;
    }
    &--1 {
        position: relative;
        font-size: calc(80 / 1920 * 100vw);

    }

    &--5 {
        position: relative;
        font-size: calc(50 / 1920 * 100vw);

    }
}

.item-button__text1,
.item-button__text2,
.item-button__text3,
.item-button__text4,
.item-button__text5,
.item-button__text6 {
    position: absolute;
}


// .item-button--2 {
//     position: relative;
// }

.item-button__bg {
    cursor: pointer;
    fill: #f9ed32;
    opacity: .35;


    &--1 {
        height: calc(150/1080 * 100vh);

    }

    &--2{
        height: calc(240/1080 * 100vh);
    }

    &--3 {
        height: calc(180/1080 * 100vh);
        transform: rotate(90deg);

    }

    &--4 {
        height: calc(220/1080 * 100vh);

    }

    &--5 {
        height: calc(180/1080 * 100vh);
    }

    &--6 {
        height: calc(220/1080 * 100vh);
        transform: rotate(40deg);
    }

}

.lightbox__iframe {
    border-radius: 12px;
    width: 85%;
    height: 85%;
}

.mobile {
    display: none;
}
.desctop {
    display: flex;
}

@media screen and (max-width: 768px) {
    .desctop {
        display: none;
    }

    .mobile {
        display: flex;
    }

    .logotype__image {
        display: none;
    }

    .main-page__content {
        flex-direction: column;
        justify-content: flex-start;
    }

    .content__left-side  {
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .content__right-side {
        align-items: center;
        justify-content: center;
    }

    .qr-code__image {
        display: flex;
        max-width: 50vw;
        width: 100%;
        max-height: 21vh;
        height: 100%;
    }

  .button {
      position: static;
      width: 100vw;
      height: 60px;
      padding: 0;
  }

  .button__text {
    margin: 0 auto;
    // font-size: calc(60 / 768 * 100vw);
    p{

        font-size: 20px !important;
    }
  }

    .button-top {
        border-radius: 0;
    }

    .button-bottom {
        margin: 80px 0 26px 0;
        border-radius: 0;
    }

    .links {
        max-width: 70vw;
        margin-top: 80px;
        display: grid;
        grid-template-columns: repeat(2, 1fr);
        gap: calc(20 / 1080 * 100vh) 0;
    }

    .link {
    cursor: pointer;

    &-wrapper {
        display: flex;
        align-items: flex-start;
        margin: 0 auto;

        &:nth-child(2) {
            position: relative;
            top: -50px;
            order: 5;
        }

        &:nth-child(3) {
            justify-content: flex-end;
            width: 100%;

            position: relative;
            top: -30px;
            order: 3;

            margin-left: 0;

        }

        &:nth-child(4) {
            justify-content: flex-start;
            width: 100%;
            position: relative;
            top: 30px;
            right: 0;
            order: 2;
        }

        &:nth-child(5) {
            justify-content: flex-start;
            width: 80%;
            position: relative;
            top: 80px;
            order: 4;

        }

        &:nth-child(6) {
            justify-content: flex-start;
            width: 100%;
            position: relative;
            top: 100px;
            right: 30px;
            order: 6;

        }
    }

    text-decoration: none;

    outline: none;

}
.item-button {
    font-size: calc(27 / 768 * 100vw);
}

.item-button__bg {

    &--1 {
        // max-width: calc(68/1080 * 100vh);
        height: calc(87/1080 * 100vh);
    }

    &--2 {
        // max-width: calc(132/1080 * 100vh);
        height: calc(146/1080 * 100vh);
    }

    &--3 {
        // max-width: calc(74/1080 * 100vh);
        height: calc(102/1080 * 100vh);
    }

    &--4 {
        // max-width: calc(104/1080 * 100vh);
        height: calc(124/1080 * 100vh);

    }

    &--5 {
        // max-width: calc(84/1080 * 100vh);
        height: calc(106/1080 * 100vh);
    }

    &--6 {
        // max-width: calc(106/1080 * 100vh);
        height: calc(136/1080 * 100vh);
    }

}

.left-side__button {
    width: calc(400 / 768 * 100vw);
    // height: calc(290 / 768 * 100vw);
}

.left-side__text {
    font-size: calc(60 / 768 * 100vw);
}

}

@media screen and (max-width: 650px) {
    .links {
        max-width: 100%;
        margin-top: 60px;
    }

    .button-bottom {
        margin: 40px 0 16px 0;
    }
}

@media (max-width: 350px) {
    .links {
        max-width: 100%;
        margin-top: 40px;
        margin-bottom: 100px;
    }

    .button-bottom {
        margin: 20px 0 8px 0;
    }

    .item-button {
    font-size: calc(50 / 768 * 100vw);
}
}

@keyframes rotate {
  to {
    transform: rotate(1turn);
  }
}

</style>
