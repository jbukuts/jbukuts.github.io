<template>
  <div class="about" :style="$isMobile() ? 'width: 95%;' : 'width: 70%;'">

    <h2><mark>About me</mark></h2>

    <div v-if="$isMobile()" >
      <img src="../assets/images/me.jpg" id="me"/>
      <div v-html="aboutMe" style="margin-top: 18px">{{this.aboutMe}}</div>
    </div>

    <div style="display: table" v-if="!$isMobile()">
      
      <div style="display: table-cell; vertical-align: top" v-html="aboutMe">
        {{this.aboutMe}}
      </div>

      <div style="background: white; height: 200px; text-align: right; width: 200px; display: table-cell; padding-left: 15px">
        <span style="display:inline-block; height: 100%; vertical-align: middle"></span>
        <img src="../assets/images/me.jpg" id="me"/>
      </div>
    </div>

    <h2><mark>Here is some info about this page</mark></h2>

    <p>This website was created by me with the use of VueJS for the frontend. Various APIs were also used to create some of the functionality of this page. Some of these include:</p>
    <ul>
      <li v-for="i in apis" :key=i[0]><a :href="i[1]" target="_blank">{{i[0]}}</a> <ul><li>{{i[2]}}</li></ul> </li>
    </ul>

    <p>Some other technologies were used in order to make this page more secure and functional. Some of those include:</p>
    <ul>
      <li v-for="i in tech" :key=i[0]><a :href="i[1]" target="_blank">{{i[0]}}</a> <ul><li>{{i[2]}}</li></ul> </li>
    </ul>

    <p>This site is still being updated to be able to accommodate other browsers. For instance, one of the idiosyncrasies of Firefox is the inability to customize the scrollbar. That being said this site looks best on Chromium based browsers for the time being.</p>

    <h2><mark>Connect</mark></h2>
    <a v-for="icon in connect" :key="icon[0]" :href="icon[1]"><v-icon :name="icon[0]"></v-icon></a>

  </div>
</template>

<script>
import VueMobileDetection from "vue-mobile-detection";
import Vue from 'vue';
import feather from 'vue-icon'

Vue.use(VueMobileDetection);
Vue.use(feather, 'v-icon')


export default {
  name : 'About',
  components: {
  },
  data() {
    return {
      connect: [
        ['github','https://github.com/jbukuts'],['linkedin','https://www.linkedin.com/in/jake-bukuts-54a426183'], ['twitter','https://twitter.com/jbukuts'], ['mail','mailto:jbukuts@gmail.com']
      ],
      apis : [
        ['NewsAPI','https://newsapi.org/','Used in creating the news carousel on the home page'],
        ['OpenPage Geocoder', 'https://opencagedata.com/', 'Used in order to determine city from latitude and longitude coordinates'],
        ['OpenWeather', 'https://openweathermap.org/', 'Used to pull current weather data based on location'],
        ['Github REST API', 'https://docs.github.com/en/rest', 'Used to get all of my project data']
      ],
      tech : [
        ['Firebase Functions','https://firebase.google.com/','Used in order to obfuscate my API keys and limit request origin in GET requests'],
        ['Vue Carousel','https://ssense.github.io/vue-carousel/','Used to create a clean mobile friendly UX for the news section'],
        ['Vue Mobile Detection','https://github.com/ajerez/vue-mobile-detection','Used to detect whether user is on mobile browser or not'],
        ['V-Tooltip','https://github.com/Akryum/v-tooltip','Used for tooltips on Projects page']
      ],
      aboutMe: "<p style='margin-top: 0px'>My name is Jake Bukuts. I am currently a Senior at the University of South Carolina and am " + 
              "on track to graudate with a Bachelor's of Computer Science in the spring of 2021.</p>"+

        "<p>I currently work as an Intern Backend Developer for the University of South Carolina's Research Cyber Infrastructure. The project I am currently working on is a computer vision project called SnowVision. "+ 
          "The goal of this project is to automate the process of matching Native American stamped pottery (sherds) with their corresponding designs that exist within our database. "+
          "My work entails the creation of a backend API that connects our frontend view with our archaeological collection of sherds.</p>"
    }
  }
}
</script>

<style scoped>

.icon:hover {
  cursor: pointer;
}

.icon {
  width: 40px; 
  display: inline; 
  margin: 0px 5px 0px 5px;
}

#me {
  max-height: 200px;
  width: 200px;
  object-fit: cover;
  object-position: 50% 20%;
  border-radius: 50%;
  filter: grayscale(100%) contrast(110%);
  vertical-align: middle;
}

mark {
  color: white;
  background: black;
  padding: 2.5px;
  border-radius: 0px;
}

.about {
  font-size: 18px;
  margin: auto;
  margin-bottom: 30px;
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
