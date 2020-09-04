<template>
  <div id="app">
    <nav class="main-nav">
      <div class="logo">Jake Bukuts' Github Site</div>
      <Burger></Burger>
    </nav>

    <SideBar>
      <ul class="sidebar-panel-nav">
        <router-link tag="li" to="/" @click.native="toggle">Home</router-link>
        <router-link tag="li" to="/resume" @click.native="toggle">Resume</router-link>
        <router-link tag="li" to="/about" @click.native="toggle">About</router-link>
        <router-link tag="li" to="/projects" @click.native="toggle">Projects</router-link>
      </ul>
    </SideBar>

    <div id="main">
      <router-view/>
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
    }
  }
  
}
</script>

<style>

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
  color: #fff;
  font-weight: bold;
}
.main-nav {
  display: flex;
  justify-content: space-between;
  padding: 0.5rem 0.8rem;
  background: rgb(124, 17, 17);
  position: fixed;
  top: 0px;
  width: 100vw;
  overflow: hidden;
  

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
  margin-top: 80px;
  width: 55%;
}

#app {
  top: 0;
  bottom: 0;
  position: fixed;
  overflow-y: scroll;
  width: 100%;

}



</style>
