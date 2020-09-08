<template>
  <div class="home">
    <h1>WELCOME TO MY GITHUB SITE</h1>

    <div id="clocks" >
      <Clock v-for="c in this.times" :key="c" :name='c' :timeZone='c' />
    </div>

    <p>This is just a simple github page to host my project list, resume, as well as some random ideas I may have.</p>
    <h2>CONTACT</h2>
    <p>Email : <a href="mailto:jbukuts@gmail.com">jbukuts@gmail.com</a></p>

    
  </div>
</template>

<script>
// @ is an alias to /src
import Clock from "../components/Clock.vue";

export default {
  name: 'Home',
  components : {
    Clock
  },
  data() {
    return {
      times : [ "Asia/Tokyo", "America/Los_Angeles","America/New_York" ] 
    }
  }
}
</script>

<style scoped>

h1, h2, p, a {
  text-align: center;
}

a {
  color: rgb(109, 0, 0);
}

#clocks {
  margin: auto;
  text-align: center;
}

p {
  font-size: 20px;
}

</style>
