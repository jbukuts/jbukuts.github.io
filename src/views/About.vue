<template>
  <div class="about">
    <h1>Here is some info about this page</h1>

    <p>This website was created by me with the use of VueJS for the frontend. Various APIs were also used to create some of the functionality of this page. Some of these include:</p>

    <ul>
      <li v-for="i in apis" :key=i[0]><a :href="i[1]" target="_blank">{{i[0]}}</a> <ul><li>{{i[2]}}</li></ul> </li>
    </ul>

    <p>Some other technologies were used in order to make this page more secure and functional. Some of those include:</p>

    <ul>
      <li v-for="i in tech" :key=i[0]><a :href="i[1]" target="_blank">{{i[0]}}</a> <ul><li>{{i[2]}}</li></ul> </li>
    </ul>
  </div>
</template>

<script>
export default {
  name : 'About',
  data() {
    return {
      apis : [
        ['NewsAPI','https://newsapi.org/','Used in creating the news carousel on the home page'],
        ['OpenPage Geocoder', 'https://opencagedata.com/', 'Used in order to determine city from latitude and longitude coordinates'],
        ['OpenWeather', 'https://openweathermap.org/', 'Used to pull current weather data based on location'],
        ['Github REST API', 'https://docs.github.com/en/rest', 'Used to get all of my project data']
      ],
      tech : [
        ['Firebase Functions','https://firebase.google.com/','Used in order to obfuscate my API keys in GET requests'],
        ['Vue Carousel','https://ssense.github.io/vue-carousel/','Used to create a clean mobile friendly UX for the news section'],
        ['Vue Mobile Detection','https://github.com/ajerez/vue-mobile-detection','Used to detect whether user is on mobile browser or not'],
        ['V-Tooltip','https://github.com/Akryum/v-tooltip','Used for tooltips on Projects page']
      ]
    }
  }
}
</script>

<style scoped>

.about {
  font-size: 18px;
}

/** outer list */
li {
  font-size: 18px;
}

/** sublist */
ul > li > ul > li {
  font-size: 16px;
}

a {
  color: black;
  font-weight: bold;
}

</style>
