<template>
    <div class="sidebar">
        <transition name="fade">
            <div class="sidebar-backdrop" @click="closeSidebarPanel" v-if="isPanelOpen"></div>
        </transition>
        <transition name="slide">
            <div v-if="isPanelOpen"
                 class="sidebar-panel">
                <slot></slot>
            </div>
        </transition>
    </div>
</template>

<script>
import { store, mutations } from '@/store.js'

export default {
    name: 'SideBar',
    methods: {
        closeSidebarPanel() {
            mutations.toggleNav();
            this.$emit('burger-click', store.isNavOpen);
        }
    },
    computed: {
        isPanelOpen() {
            return store.isNavOpen
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.slide-enter-active,
.slide-leave-active {
    transition: transform 0.2s ease;
}
.slide-enter,
.slide-leave-to {
    transform: translateX(-100%);
    transition: all 150ms ease-in 0s
}
.sidebar-backdrop {
    background-color: rgba(51, 51, 51, 0.35);
    width: 100vw;
    height: 100vh;
    position: fixed;
    z-index:  1 !important;
    top: 0;
    left: 0;
    cursor: pointer;
    backdrop-filter: blur(5px);
}
.sidebar-panel {
    overflow-y: auto;
    background-color: rgb(0, 0, 0);
    position: fixed;
    left: 0;
    top: 0;
    height: 100vh;
    z-index: 999;
    padding: 3rem 20px 2rem 20px;
    width: 150px;
}
</style>
