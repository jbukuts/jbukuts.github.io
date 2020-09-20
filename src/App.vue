<template>
  <div id="app">
    <nav class="main-nav">
      <div class="logo"><i>Bukuts' Blog</i></div>
      <Burger></Burger>
    </nav>

    <SideBar v-on:burger-click='toggleBlur'>
      <ul class="sidebar-panel-nav">
        <router-link tag="li" to="/" @click.native="toggle">Home</router-link>
        <router-link tag="li" to="/about" @click.native="toggle">About</router-link>
        <router-link tag="li" to="/resume" @click.native="toggle">Resume</router-link>
        <router-link tag="li" to="/projects" @click.native="toggle">Projects</router-link>
      </ul>
    </SideBar>

    <div id="main">
      <transition name="fade" mode="out-in">
        <router-view/>
      </transition>
    </div>

    <Footer/>

  </div>
</template>

<script>
import { mutations } from "@/store.js";
import SideBar from '@/components/SideBar.vue'
import Burger from "./components/Burger.vue";
import Footer from "./components/Footer.vue";




export default {
  name: 'App',
  components: {
    SideBar,
    Burger,
    Footer
  },
  methods: {
    toggle() {
      console.log("toggle");
      mutations.toggleNav();
    },
    toggleBlur(blur) {
      console.log(blur);
      document.getElementById('main').style.filter = (blur == true) ? 'blur(2px)' : 'blur(0px)';

    }
  }
  
}
</script>

<style>

.VueCarousel-dot:focus {
  outline: 0px solid black !important;
}

.router-link-exact-active {
  text-decoration: underline !important;
}

@font-face {
  font-family: Futura;
  src: url(Futura.ttf);
}

html {
  height: 100%;
}
body {
  border: 0px;
  margin: 0px;
  padding: 0px;
  font-family: 'Futura';
  height: 100%;
  background: white;
}

.logo {
  align-self: center;
  font-size: 22px;
  color: #fff;
  font-weight: bold;
}
.main-nav {
  display: flex;
  justify-content: space-between;
  padding: 0.5rem 0.8rem;
  position: fixed;
  top: 0px;
  width: 100vw;
  overflow: hidden;
  z-index: 2;

  background: linear-gradient(270deg, #ff0000, #d58516, #aeb728, #00813e, #003681, #b757c1);
  background-size: 1200% 1200%;
  -webkit-animation: AnimationName 20s ease infinite;
  animation: AnimationName 20s ease infinite;
}
ul.sidebar-panel-nav {
  list-style-type: none;
}
ul.sidebar-panel-nav > li {
  color: #fff;
  text-decoration: none;
  font-size: 1.5rem;
  display: block;
  padding-bottom: 0.5em;
}

ul.sidebar-panel-nav > li:hover {
  text-decoration: underline;
  cursor: pointer;
}

#main {
  margin-left: auto;
  margin-right: auto;
  width: 90%;
  height: calc(100% - 71px);
  padding-top: 46px;
  padding-bottom: 25px;
  overflow-x: hidden;
  overflow-y: scroll;
}

::-webkit-scrollbar {
  display: none;
}

#app {
  top: 0;
  bottom: 0;
  position: fixed;
  width: 100%;
  overflow: hidden;
}

.tooltip {
  display: block !important;
  z-index: 10000;
}

.tooltip .tooltip-inner {
  background: black;
  color: white;
  border-radius: 5px;
  padding: 3px;
}

.tooltip .tooltip-arrow {
  width: 0;
  height: 0;
  border-style: solid;
  position: absolute;
  margin: 5px;
  border-color: black;
  z-index: 1;
}

.tooltip[x-placement^="bottom"] .tooltip-arrow {
  border-width: 0 5px 5px 5px;
  border-left-color: transparent !important;
  border-right-color: transparent !important;
  border-top-color: transparent !important;
  top: -5px;
  left: calc(50% - 5px);
  margin-top: 0;
  margin-bottom: 0;
}



.tooltip[aria-hidden='true'] {
  visibility: hidden;
  opacity: 0;
  transition: opacity .15s, visibility .15s;
}

.tooltip[aria-hidden='false'] {
  visibility: visible;
  opacity: 1;
  transition: opacity .15s, visibility .15s;
}

.fade-enter-active,
.fade-leave-active {
  transition-duration: 0.3s;
  transition-property: opacity;
  transition-timing-function: ease;
}

.fade-enter,
.fade-leave-active {
  opacity: 0
}



@-webkit-keyframes AnimationName {
    0%{background-position:0% 50%}
    50%{background-position:100% 50%}
    100%{background-position:0% 50%}
}
@keyframes AnimationName {
    0%{background-position:0% 50%}
    50%{background-position:100% 50%}
    100%{background-position:0% 50%}
}


</style>
