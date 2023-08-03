<template>
    <header class="header">
        <div class="icon">
            <h2 @click="toggleDropdown">Click</h2>
            <dialog class="dialog" ref="dialog" style="padding: 0" @click="$event.target === dialog && dialog?.close()">
                <section class="dialog-content">
                    <div>One</div>
                    <div>Two</div>
                </section>
            </dialog>
        </div>
    </header>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const isDropdownOpen = ref(false)
// const input = ref<HTMLInputElement | null>(null)
const dialog = ref<HTMLDialogElement | null>(null)

const toggleDropdown = () => {
    // dialog.value?.showModal()
    dialog.value?.open ? dialog.value?.close() : dialog.value?.show()
    isDropdownOpen.value = !isDropdownOpen.value
}

</script>

<style lang="scss">
.header {
    width: 100%;
    height: 70px;
    background-color: antiquewhite;

    .icon {
        position: relative;
        width: max-content;
        padding: 1rem 3rem;
        // margin-inline-start: 2rem;

        h2 {
            margin: 0;
        }
    }

    .dialog {
        background-color: #fff;
        box-shadow: 0 0 4px 1px rgba(0, 0, 0, 0.203);
        border: none;
        outline: none;
        // position: absolute;
        top: 110%;
        z-index: 100;
        // position: relative;
        // left: 20px;
        &::after {
            content: "";
            width: 20px;
            height: 20px;
            rotate: 45deg;
            position: absolute;
            top: 0;
            left: 50%;
            translate: -50% -50%;
            // background-color: gold;
            background-color: #fff;
            box-shadow: -2px -2px 3px 0 rgba(0, 0, 0, 0.203);
        }


        .dialog-content {
            padding: 1rem;

            div {
                cursor: pointer;
                padding: 6px 12px;

                &:hover {
                    background-color: rgba(0, 0, 0, 0.119);
                }
            }
        }
    }
}
</style>