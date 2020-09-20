<template>
  <div class="about">
    <h1>Here are some of the technologies used in this page</h1>
    <ul>
      <li v-for="i in tech" :key=i>{{i}}</li>
    </ul>
  </div>
</template>

<script>
export default {
  name : 'About',
  data() {
    return {
      tech : ['Vuejs','NewsApi','FireBase Functions', 'Github REST API']
    }
  }
}
</script>
