<template>
  <div class="home">
    <h1>Weclome to my Github Site!</h1>

    <div id="clocks" >
      <Clock v-for="c in this.times" :key="c" :name='c' :timeZone='c' />
    </div>

    
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
h1 {
  text-align: center;
}

#clocks {
  margin: auto;
  text-align: center;
}

</style>
