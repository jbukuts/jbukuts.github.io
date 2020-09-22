<template>
  <div class="home" :style="this.loaded == false ? 'overflow : hidden' : ''">

    <Loader v-if="this.loaded == false"/>

    <div v-if="this.loaded == true">
      <div id="header-stuff">
        <h1>welcome to my <mark>Github</mark> Page</h1>
        <h1>today is <mark>{{`${date.toLocaleString('default', { month: 'short' })} ${date.getDate()}, ${date.getFullYear()}`}}</mark></h1>
        <h1>it's day <mark>{{dayAroundSun()}}</mark> around the sun</h1>
        <h1>in <mark>{{location.city}}, {{location.state}}</mark> it's <mark>{{Math.round(dailyForecast.current.temp)}}°</mark></h1>
      </div>
      
      <!--<div id="clocks" >
        <Clock v-for="c in this.times" :key="c" :name='c' :timeZone='c' />
      </div>-->

      <hr style="margin-bottom: 30px">

      <div class="cover">
      <carousel :perPage=3 :perPageCustom="[[0, 1], [605, 2], [768, 3], [1200,4]]" :loop=true :autoplay=true :autoplayTimeout=5000 :paginationEnabled='!$isMobile()'>
        <slide  v-for="art in this.articles" :key="art.title">
          <div class="article">
            <h3 style="text-align: right">
              <mark>{{art.title.substring(0,art.title.lastIndexOf(" - "))}}</mark>
            </h3>

            <p v-if="art.author != null && art.author != ''" style="text-align: left; font-size: 12px; margin-bottom: 0px">
              By <b>{{art.author.toUpperCase()}}</b>
            </p>

            <p style="text-align: left; font-size: 12px; margin-top: 0px">
              Published at {{dateToString(art.publishedAt)}}
            </p>

            <img :src='art.urlToImage' style="border-radius: 10px; filter: grayscale(100%); max-height: 140px; max-width: 90%" />
            <p style="font-size: 14px">{{art.description}}</p>
            <a :href="art.url" target="_blank" style="color: black; font-weight: bold; font-size: 14px; position: absolute; bottom: 5px; left: calc(50% - 6.5em)">CLICK TO READ FULL ARTICLE</a>
          </div>
        </slide>
      </carousel>
      </div>

      <hr style="margin-top:30px">
      <p style="text-align:right; font-size: 10px">created with NewsAPI</p>
    </div>

  </div>
</template>

<script>
// @ is an alias to /src
//import Clock from "../components/Clock.vue";
import axios from 'axios';
import { Carousel, Slide } from 'vue-carousel';
import Loader from '../components/Loader.vue';

export default {
  name: 'Home',
  components : {
    Carousel,
    Slide,
    Loader,
    //Clock
  },
  data() {
    return {
      times : [ "Asia/Tokyo", "America/Los_Angeles","America/New_York" ] ,
      articles : [],
      date : new Date(),
      location : {city : 'Charlotte', state : 'NC'},
      dailyForecast : null,
      geoCodeKey : '532ecae0e6264ea5876b01e53a23605a',
      apiKey : 'dfdc47588066cf48c7e29192f8c86c74',
      loaded : false,
    }
  },
  methods : {
    async animateHeader() {

      // this animation only occurs on first load of the page
      let firstLoad = localStorage.getItem('firstLoad') == null ? true : false; 
      let diff = 250;
      let header = document.getElementById('header-stuff').querySelectorAll('h1');
      let theRest = document.querySelector('.home').children[0].childNodes;

      // load in header
      for (let i=0; i<header.length; ++i) {
        header[i].style.opacity = 1;
        if (firstLoad)
          await new Promise(r => setTimeout(r, diff));
      }

      // load the rest
      for (let i=0; i<theRest.length; ++i) {
        theRest[i].style.opacity = 1;
        if (firstLoad)
          await new Promise(r => setTimeout(r, diff));
      }

      // time to set to not null
      if (firstLoad)
        localStorage.setItem('firstLoad','');
    },
    dayAroundSun() {
      var start = new Date(this.date.getFullYear(), 0, 0);
      var diff = this.date - start;
      var oneDay = 1000 * 60 * 60 * 24;
      return Math.floor(diff/oneDay);
    },
    dateToString(d) {
      var date = new Date(d);
      return `${date.toLocaleString('default', { month: 'short' })} ${date.getDate()}, ${date.getFullYear()} `;
    },
    async getArticles() {
      return await axios.get('https://us-central1-vcrhomepage-8711c.cloudfunctions.net/getTopHeadlines').then((d) => {
        return d.data.articles.filter(x => x.description != null);
      });
    },
    async getForecast(lat, lng) {
      let weatherURL = `https://api.openweathermap.org/data/2.5/onecall?lat=${lat}&lon=${lng}&appid=${this.apiKey}&units=imperial`;
      return await axios.get(weatherURL).then(res => {
          return res.data;
      });
    },
    async getLocation(lat,lng) {
      let geoURL = `https://api.opencagedata.com/geocode/v1/json?q=${lat}+${lng}&key=${this.geoCodeKey}`;
      return await axios.get(geoURL).then(res => {
        // we only need these two things from the api
        return {
          city : res.data.results[0].components.city,
          state : res.data.results[0].components.state_code
        }
      });
    }
  },
  async mounted() {

    // arr to hold arr promises
    let promArr = [this.getArticles()];

    if (navigator.geolocation) {    
      navigator.geolocation.getCurrentPosition(async (loc) => {
        let lat = loc.coords.latitude;
        let lng = loc.coords.longitude;        
        promArr.push(this.getForecast(lat, lng));
        promArr.push(this.getLocation(lat, lng));

        // resolve all promises and set values
        Promise.all(promArr).then((values) => {
          this.articles = values[0];
          this.dailyForecast = values[1];
          this.location = values[2];
        }).then(async () => {
          // time to load the page
          // wait 500 ms so that animation works properly
          this.loaded = true;
          await new Promise(r => setTimeout(r, 1));
          this.animateHeader();
          console.log(this.dailyForecast);
        });
      }); 
    }
  }
}
</script>

<style scoped>

/** for the incremental fade in animation */
.cover, hr, .home > div > p, #header-stuff h1  {
  opacity: 0;
  transition: .65s opacity;
}

#header-stuff {
  margin-top: 10px;
  margin-bottom: 10px;
}

#header-stuff h1 {
  text-align: left;
  margin-top: 0px;
  margin-bottom: -3px;
  font-size: 26px
}

mark {
  color: white;
  background: rgb(0, 0, 0);
  padding-left: 7px;
  padding-right: 7px;
}

.home {
  height: 100%;
}

.article {
  border: 1px solid black;
  margin: auto;
  padding: 0px;
  text-align: center;
  padding: 10px;
  height: 480px;
  background: white;
  position: relative;
  margin-right: 10px;
  margin-left: 10px;
  margin-bottom: 10px;
}

.article::after {
  content: "";
  position: absolute;
  z-index: -4;
  bottom: -10px;
  left: -10px;
  height: 100%;
  width: 100%;
  opacity: 0.7;
  /* Declaring our shadow color inherit from the parent (button) */
  background: linear-gradient(270deg, #ff0000, #d58516, #aeb728, #00813e, #003681, #b757c1);
  background-size: 1200% 1200%;
  -webkit-animation: AnimationName 20s ease infinite;
  animation: AnimationName 20s ease infinite;
}

h1, h2, p, a {
  text-align: center;
}

#clocks {
  margin: auto;
  text-align: center;
}

.article a:hover {
  text-decoration: underline;
  text-decoration-style: dashed;
}

p {
  font-size: 20px;
}

</style>
