<template>
    <section class="carousel-wrapper"
        :style="{ '--dir': dir, '--trans': `${100 * dir}%`, '--show': `${100 * dir * -1}%`, '--hide': `${100 * dir}%` }">
        <ul class="clean-list flxex">
            <li v-for="(img, idx) in imgsFormat" :key="idx" ref="imgWrapper">
                <img class="carousel-img" ref="img" :src="img.url"
                    :class="{ show: currIdx === idx, hide: currIdx !== idx }">
            </li>
        </ul>
        <div class="btns flex">
            <div v-for="(img, idx) in imgsFormat" :key="img.url">
                <div @click="selectImg(idx)" class="btn" :class="{ 'curr-btn': currIdx === idx }">
                    <!-- {{ idx + 1 }} -->
                </div>
            </div>
        </div>
        <div class="arrows flex space-between">
            <div class="arrow" @click="toLeft()">
                <svg style="rotate: 90deg" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
                    <path d="m12 15.375-6-6 1.4-1.4 4.6 4.6 4.6-4.6 1.4 1.4Z" fill="black" />
                </svg>
            </div>
            <div class="arrow" @click="toRight()">
                <svg style="rotate: 270deg" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
                    <path d="m12 15.375-6-6 1.4-1.4 4.6 4.6 4.6-4.6 1.4 1.4Z" fill="black" />
                </svg>
            </div>
        </div>
    </section>
</template>

<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted } from 'vue'

interface Img {
    url: string
}

interface Props {
    imgs: Img[]
    isAuto: boolean
    interval: number
    transition: number
}

const { isAuto, imgs, interval, /* transition */ } = withDefaults(defineProps<Props>(), {
    isAuto: false,
    imgs: () => [],
    interval: 4000,
    transition: 0.3
})

const currIdx = ref(0)
const dir = ref(1)

const img = ref(null)
const imgWrapper = ref(null)
const exImages: Img[] = [
    { url: 'https://res.cloudinary.com/dvirco123/image/upload/v1685443927/sax_2_pzl40q.png' },
    // { url: 'https://res.cloudinary.com/dvirco123/image/upload/v1686137184/Keeper/me_tjm1zw.jpg' },
    { url: 'https://res.cloudinary.com/dvirco123/image/upload/v1686841640/Keeper/CRM_UI_UX_2_mavncv.png' },
    // { url: 'https://res.cloudinary.com/dvirco123/image/upload/v1684320021/logo-misterbit_fprccf.png' },
    { url: 'https://res.cloudinary.com/dvirco123/image/upload/v1684320286/%D7%A6%D7%99%D7%9C%D7%95%D7%9D_%D7%9E%D7%A1%D7%9A_2022-03-22_234412_ag8ebf.png' },
]

const imgsFormat = computed(() => imgs?.length ? imgs : exImages )

// const imgsFormat = computed<Img[]>(() => props.imgs?.length ? props.imgs : exImages)
// const showDir = computed(() => `${-100 * dir.value/*  * -1 */}%` )
// const hideDir = computed(() => `${100 * dir.value}%` )

// const transitionTime = computed(() => `${transition}s` )

let intervalId!: number

onMounted(() => {
    if (isAuto) intervalId = setInterval(() => {
        toRight()
    }, interval)
})

onUnmounted(() => {
    clearInterval(intervalId)
})

const toLeft = () => {
    dir.value = 1
    if (!currIdx.value) {
        currIdx.value = imgsFormat.value.length - 1
    }
    else currIdx.value--
}
const toRight = () => {
    dir.value = -1
    if (currIdx.value === imgsFormat.value.length - 1) {
        currIdx.value = 0
    }
    else currIdx.value++
}

const selectImg = (idx: number) => {
    dir.value = currIdx.value > idx ? 1 : -1
    currIdx.value = idx
}

</script>

<style lang="scss">
.carousel-wrapper {
    position: fixed;
    top: 50%;
    left: 50%;
    translate: -50% -50%;

    width: 600px;
    height: 400px;
    background-color: gray;
    overflow: hidden;
    border-radius: 10px;

    ul {
        flex-wrap: nowrap;
    }

    .carousel-img {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        object-fit: cover;
        object-position: center;

        // &.show {

        //     animation: show v-bind(transitionTime) ease-in-out;
        // }

        // &.hide {
        //     opacity: 0;
        //     animation: hide v-bind(transitionTime) ease-in-out;
        // }
    }
}

.btns {
    position: absolute;
    top: 90%;
    left: 50%;
    translate: -50% ;
    z-index: 10;
    gap: 6px;

    .btn {
        cursor: pointer;
        background-color: #ffffff7e;
        border-radius: 50%;
        width: 10px;
        height: 10px;
        
        &.curr-btn {
            background-color: white;
        }
    }
}

.arrows {
    position: absolute;
    top: 50%;
    translate: 0 -50%;
    width: 100%;

    .arrow {
        width: 60px;
        height: 60px;
        cursor: pointer;
        border-radius: 50%;
        
        svg {
            path {
                fill: white;
            }
        }

        &:hover {
            background-color: rgba(0, 0, 0, 0.12);
        }
    }
}
</style>