<script setup>
import GoHome from "@/components/go-home.vue";
import {computed, ref, toRefs} from "vue";
import Lightbox from "@/components/Lightbox.vue";
import {useRoute} from "vue-router";
import ind_names from "@/stores/indust-names";
import {useAFKStore} from "@/stores/afk.js";
import Vote from "@/components/vote.vue";
import public_content from "@/stores/public_content.js"

const $route = useRoute();
const russia_logo = new URL(`@/assets/icons/russia-logo.svg`, import.meta.url);

// let image_url = ref(new URL(`@/assets/images/screens/fon.png`, import.meta.url));
// let image_url = ref(new URL(`@/assets/images/screens/videos-grey.svg`, import.meta.url));
let icon_name = ref('home-white');
let icon_class = ref('');
let icon_text = ref('на главную');
const is_pdf_in_lightbox = ref(false);
if ($route.params.video_id[2] !== undefined) {
    image_url.value = new URL(`@/assets/images/screens/ind-vid.svg`, import.meta.url);
    icon_name.value = 'back-arrow';
    icon_class.value = 'arrow';
    icon_text.value = 'назад'
}
const is_lightbox = ref(false);
const parsedContentURL = ref(null);

// потом заменить 2 на максимальное кол-во видео
// const count_of_objects = computed(() => $route.params.video_id[0] === '1' ? 2 : $route.params.video_id[0] === '2' ? countOfSecondV() : 14);
const count_of_objects = computed(() => Object.keys(public_content[$route.params.video_id].objects).length);
console.log(count_of_objects)
// const count_of_pdf = computed(() => $route.params.video_id[0] === '1' ? 2 : $route.params.video_id[0] === '2' ? 1 : 1);
// const title_of_page = computed(() => {
//     const title = $route.params.video_id[0] === '1' ? 'Спорт' :
//                   $route.params.video_id[0] === '2' ? 'Вайб</br>Владикавказа':
//                   $route.params.video_id[0] === '3' ? 'Для</br>молодёжи' :
//                   $route.params.video_id[0] === '4' ? 'Культурная</br>жизнь':
//                   $route.params.video_id[0] === '5' ? 'Путешествия</br>в Осетию':
//                   $route.params.video_id[0] === '6' ? 'Рост</br>производства': null;
//     if (title) return title;
//     else return ind_names[+($route.params.video_id[2] + ($route.params.video_id[3] !== undefined ? $route.params.video_id[3] : '')) - 1];
// });


function countOfSecondV() {
    switch ($route.params.video_id[2] + ($route.params.video_id[3] !== undefined ? $route.params.video_id[3] : '')) {
        case '1':
        case '7':
            return 2;
        case '3':
        case '4':
        case '5':
        case '6':
            return 3;
        case '10':
        case '8':
        case '9':
            return 1;
        case '11':
            return 4;
        case '2':
            return 5;
    }
}

function showLightbox(item) {
    console.log(item)
    const is_pdf_format = public_content[$route.params.video_id][item].is_pdf || false;
    let content_name = "video.mp4";
    if(is_pdf_format){
        is_pdf_in_lightbox.value = true;
        content_name = 'presentation.pdf';
    }
    else {
        is_pdf_in_lightbox.value = false;
    }
    const string_url = `/videos/${$route.params.video_id}/${item}/${content_name}`;
    console.log("THE STRING URL");
    parsedContentURL.value = new URL(string_url, import.meta.url);
    is_lightbox.value = true;
}

function showPresentation() {
    is_pdf_in_lightbox.value = true;
    is_lightbox.value = true

}

function closeLightbox() {
    is_lightbox.value = false;
    enableAFK();
}
function parseObjectDescription(item){
    return public_content[+$route.params.video_id]['objects'][item].description;
}
function parsePageText(item){
    return public_content[+$route.params.video_id].text;

}
function parseImageURL(item) {
    let additional = $route.params.video_id[2] + ($route.params.video_id[3] !== undefined ? $route.params.video_id[3] : '');
    if (additional !== 'undefined') additional = additional + '/';
    else additional = ''
    const string_url = `/videos/${$route.params.video_id[0]}/${additional}${item}/preview.jpg`;
    return new URL(string_url, import.meta.url);
}


const afk_store = useAFKStore();

function disableAFK() {
    afk_store.is_afk_watcher_enabled = false;
}

function enableAFK() {
    afk_store.is_afk_watcher_enabled = true;
}
</script>

<template>
    <main class="videos-page page">
        <img :src="russia_logo" alt="" class="videos-page__russia-logo">
        <vote class="videos-page__ossetia-logo"/>

        <img :class="{original: $route.params.video_id[2] !== undefined}" :src="image_url"
             class="videos-page__background">
        <div class="videos-page__content">
            <go-home :content="icon_text" :class="icon_class" :icon_name="icon_name"
                     :to="$route.params.video_id[0] === '2'?'/':'/'"
                     class="videos-page__home"/>
            <h1 class="videos-page__title" v-html="title_of_page"></h1>
            <div class="content-block">
            <div class="videos-page__videos">
                <div v-for='item in count_of_objects' class="video-backing">
                    <div @click="showLightbox(item)" class="video">
                        <img :src="parseImageURL(item)" alt="" class="video__image">
                    </div>
                    <div class="video-text">{{ parseObjectDescription(item) }}</div>
                </div>
                <div  @click="showPresentation()" class="video">
                    <img src="../assets/images/screens/videos-grey.svg">
                    <!-- ../assets/images/screens/videos-grey.svg -->
                    <!-- preview pr -->
                </div>

            </div>
            <div class="information-content">
                <div class="information-text">
                    {{ parsePageText() }}
                </div>
            </div>

        </div>
        </div>
        <transition name="modal-transition">
            <Lightbox class="lightbox" @close="closeLightbox" v-if="is_lightbox">
                <video v-if="!is_pdf_in_lightbox" @pause="enableAFK" @play="disableAFK" controls autoplay
                       class="lightbox-video"
                       :src="parsedContentURL"/>
                    <iframe v-else :src="parsedContentURL" class="lightbox-iframe"></iframe>
            </Lightbox>
        </transition>
                <!-- <iframe v-else src="/iframe-pdf.pdf#toolbar=0" class="lightbox-iframe"></iframe> -->
    </main>
</template>


<style lang="scss" scoped>
.videos-page {
    height: 100%;
    display: flex;
    align-items: stretch;
    justify-content: flex-start;
    flex-direction: column;
    position: relative;

    &__ossetia-logo {
        position: absolute;
        right: 0;
        top: calc(-50 / 1080 * 100vh);
        height: calc(300 / 1920 * 100vw);
        width: calc(375 / 1920 * 100vw);
    }

    &__russia-logo {
        position: absolute;
        top: calc(-40 / 1080 * 100vh);
        height: calc(200 / 1920 * 100vw);
        width: calc(275 / 1920 * 100vw);
        margin-bottom: calc(27 / 1080 * 100vh);
    }

    &__background {
        // background-color: $grey;
        // background: url('@/assets/images/screens/fon.png') center no-repeat;
        position: fixed;
        top: -30vh;
        left: -30vw;
        width: 150vw;
        height: 150vh;
        object-fit: cover;
        display: block;
        z-index: -1;

        &.original {
            top: -3px;
            left: -3px;
            width: 101vw;
            height: 101vh;
        }
    }

    &__home {
        fill: white !important;
        color: white !important;
        align-self: flex-start;
        margin-bottom: 20px;

        &.arrow {
            align-items: center;
        }
    }

    &__content {
        display: flex;
        align-items: center;
        justify-content: flex-start;
        flex-direction: column;
        padding-bottom: 30px;

    }

    &__title {
        font-family: 'Rumpelstiltskin', sans-serif;
        display: flex;
        align-items: center;
        justify-content: flex-start;
        flex-grow: 1;
        flex-shrink: 0;
        text-align: center;
        height: calc(300 / 1920 * 100vw);
        font-size: calc(100 / 1920 * 100vw);
        color: white;
        margin-bottom: calc(40 / 1080 * 100vh);
        line-height: 100%;

        &__screen-buttons {
            flex-grow: 1;
            flex-shrink: 0;
            display: flex;
            flex-direction: column;
            max-width: calc(580 / 1920 * 100vw);
            gap: 10px;
        }
    }

    &__videos {
        flex-grow: 1;
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 10px;
    }
}

.content-block {
    display: flex;
    align-items: flex-start;
}

.video {
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
    overflow: hidden;
    aspect-ratio: 19/10;
    border-top-left-radius: 12px;
    border-top-right-radius: 12px;
    border-bottom-right-radius: 12px;
    border-bottom-left-radius: 40px;
    cursor: pointer;
    transition: .25s;
    z-index: 10;
    &:hover {
        transform: scale(1.03);

    }

    &__image {
        display: block;
        width: 100%;
        height: 100%;
        object-fit: fill;
        transition: .25s;
    }
}

.video-backing {
    position: relative;
    z-index: 9;
    border-top-left-radius: 12px;
    border-top-right-radius: 12px;
    border-bottom-left-radius: 40px;
    border-bottom-right-radius: 12px;
    background-color: rgba(255,255,255,0.72);
    padding: 2px;
}

.video-text {
    font-family: 'OpenSans', sans-serif;
    text-align: end;
    text-transform: uppercase;
    padding: 16px 30px;
}

.information-content {
    background-color: rgba(255,255,255,0.72);
    border-top-left-radius: 24px;
    border-top-right-radius: 24px;
    border-bottom-left-radius: 48px;
    border-bottom-right-radius: 24px;
    padding: 80px 40px 80px;
    max-width: 800px;
    margin-left: 60px;
}

.information-text {
    font-family: 'OpenSans', sans-serif;
    font-size: 20px;
    line-height: 130%;

}

.lightbox {
    z-index: 1000;
}

.lightbox-video {
    width: 100%;
    max-width: 88%;
    height: auto;
    max-height: 88%;
    border-radius: 12px;
    box-shadow: 0 0 10px 1px black;
}
.lightbox-iframe {
    width: 100%;
    max-width: 88%;
    height: 100%;
    max-height: 88%;
    border-radius: 12px;
    box-shadow: 0 0 10px 1px black;
}

@media (max-width: 768px) {

    .videos-page__ossetia-logo {
        height: calc(250 / 768 * 100vw);
        width: calc(325 / 768 * 100vw);
    }

    .videos-page__russia-logo {
        top: calc(-15 / 1080 * 100vh);
        height: calc(150 / 768 * 100vw);
        width: calc(225 / 768 * 100vw);
    }

    .videos-page__videos {
        grid-template-columns: repeat(1, 1fr);
        gap: 20px;
        margin: 0 10px;
    }
    .videos-page__title {
        font-size: calc(100 / 768 * 100vw);
        margin-top: calc(60 / 1080 * 100vh);

    }

    .video-text {
        padding: 16px 16px;
    }

    .information-content {
        margin: 20px 10px;
        padding: 40px 20px 40px;

    }

    .content-block {
        flex-direction: column;
    }
}
</style>
