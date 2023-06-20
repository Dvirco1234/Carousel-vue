<template>
    <section class="carousel-wrapper"
        :style="{ '--dir': dir, '--trans': `${100 * dir}%`, '--show': `${100 * dir * -1}%`, '--hide': `${100 * dir}%` }">
        <ul class="clean-list flxex">
            <li v-for="(img, idx) in imgsFormat" :key="idx" ref="imgWrapper">
                <img class="carousel-img" ref="img" :src="img.url"
                    :class="{ show: currIdx === idx, hide: currIdx !== idx, 'hide-before': idx < currIdx, 'hide-after': idx > currIdx, first: !idx, last: idx === imgs.length - 1, 'no-trasition': isEnd }">
            </li>
        </ul>
    </section>
    <div class="flex">
        <div v-for="(img, idx) in imgsFormat" :key="img.url">
            <button @click="selectImg(idx)" :class="{ 'curr-btn': currIdx === idx }">{{ idx + 1 }}</button>
        </div>
    </div>
    <div class="arrows flex">
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
    {{ currIdx }}
</template>

<script setup lang="ts">
interface Img {
    url: string
}

import { ref, computed } from 'vue'
// const props = defineProps({
//     imgs: {
//         type: Array,
//         default: () => []
//     },
//     isAuto: {
//         type: Boolean,
//         default: false,
//     },
// })

// const props = defineProps<{
//     imgs: Img[],
//     isAuto: {
//         type: Boolean,
//         default: false,
//     },
// }>()
export interface Props {
    imgs: Img[]
    isAuto: boolean
}

const props = withDefaults(defineProps<Props>(), {
    isAuto: false,
    imgs: () => []
})

const currIdx = ref(0)
const dir = ref(1)
const isEnd = ref(false)
const img = ref(null)
const imgWrapper = ref(null)
const exImages: Img[] = [
    { url: 'https://res.cloudinary.com/dvirco123/image/upload/v1685443927/sax_2_pzl40q.png' },
    // { url: 'https://res.cloudinary.com/dvirco123/image/upload/v1686137184/Keeper/me_tjm1zw.jpg' },
    { url: 'https://res.cloudinary.com/dvirco123/image/upload/v1686841640/Keeper/CRM_UI_UX_2_mavncv.png' },
    // { url: 'https://res.cloudinary.com/dvirco123/image/upload/v1684320021/logo-misterbit_fprccf.png' },
    { url: 'https://res.cloudinary.com/dvirco123/image/upload/v1684320286/%D7%A6%D7%99%D7%9C%D7%95%D7%9D_%D7%9E%D7%A1%D7%9A_2022-03-22_234412_ag8ebf.png' },
]

// const imgsFormat = computed(() => props.imgs?.length ? props.imgs : exImages )

const imgsFormat = computed<Img[]>(() => props.imgs?.length ? props.imgs : exImages)

const toLeft = () => {
    console.log('left')
    dir.value = 1
    if (!currIdx.value) {
        currIdx.value = imgsFormat.value.length - 1
    }
    else currIdx.value--
}
const toRight = () => {
    console.log('right')
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

        &.show {
            animation: show .3s ease-in-out;
        }

        &.hide {
            opacity: 0;
            animation: hide .3s ease-in-out;
        }
    }
}

.curr-btn {
    background-color: brown;
}

@keyframes show {
    0% {
        opacity: 1;
        translate: var(--show);
    }

    100% {
        translate: 0;
    }
}

@keyframes hide {
    0% {
        opacity: 1;
        translate: 0;
    }

    99% {
        opacity: 1;
        translate: var(--hide);
    }

    100% {
        opacity: 0;
        translate: 0;
    }
}


// @keyframes show {
//     0% {
//         opacity: 1;
//         translate: var(--show);
//     }
//     99% {
//         opacity: 1;
//         translate: 0;
//     }
//     100% {
//         opacity: 0;
//     }
// }

.arrows {
    position: absolute;
    top: 80%;
    left: 50%;
    translate: -50%;

    .arrow {
        width: 60px;
        height: 60px;
        cursor: pointer;

        &:hover {
            background-color: rgba(0, 0, 0, 0.12);
        }
    }
}
</style>