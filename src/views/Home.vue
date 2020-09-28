<template>
  <div class="home" :style="!this.loaded ? 'overflow : hidden' : ''">

    <Loader v-if="!this.loaded"/>

    <div v-if="this.loaded == true">
      <div id="header-stuff">
        <h1>welcome to my <mark>Github</mark> Page</h1>
        <h1>today is <mark>{{`${date.toLocaleString('default', { month: 'short' })} ${date.getDate()}, ${date.getFullYear()}`}}</mark></h1>
        <h1>it's day <mark>{{dayAroundSun()}}</mark> around the sun</h1>
        <h1 v-if="this.locationOn">in <mark>{{location.city}}, {{location.state}}</mark> it's <mark>{{Math.round(dailyForecast.current.temp)}}°</mark></h1>
      </div>
      
      <!--<div id="clocks" >
        <Clock v-for="c in this.times" :key="c" :name='c' :timeZone='c' />
      </div>-->

      <hr style="margin-bottom: 30px">

      <div class="cover">
      <carousel :perPage=3 :perPageCustom="[[0, 1], [605, 2], [768, 3], [1200,4]]" :loop=true :autoplay=true :autoplayTimeout=5000 :paginationEnabled='!$isMobile()'>
        <slide  v-for="art in this.articles" :key="art.title">
          <Article :art='art'/>
        </slide>
      </carousel>
      </div>

      <hr style="margin-top:30px">
      <p style="text-align:right; font-size: 10px">created with NewsAPI</p>

      <div class="weather" v-if="locationOn">
        <WeatherCard :v-for="fore in this.dailyForecast.daily" :key="fore" :card="fore"/>
      </div>
      
    </div>

  </div>
</template>

<script>
// @ is an alias to /src
//import Clock from "../components/Clock.vue";
import axios from 'axios';
import { Carousel, Slide } from 'vue-carousel';
import Article from '../components/Article.vue';
import Loader from '../components/Loader.vue';
import WeatherCard from '../components/WeatherCard.vue';

export default {
  name: 'Home',
  components : {
    Carousel,
    Slide,
    Loader,
    Article,
    WeatherCard
    //Clock
  },
  data() {
    return {
      times : [ "Asia/Tokyo", "America/Los_Angeles","America/New_York" ] ,
      articles : [],
      date : new Date(),
      location : {city : 'Charlotte', state : 'NC'},
      dailyForecast : null,
      loaded : false,
      locationOn : false
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
    async getArticles() {
      return await axios.get('https://us-central1-vcrhomepage-8711c.cloudfunctions.net/getTopHeadlines').then((d) => {
        return d.data.articles.filter(x => x.description != null);
      });
    },
    async getForecast(lat, lng) {
      let weatherURL = `https://us-central1-vcrhomepage-8711c.cloudfunctions.net/getForecast?lat=${lat}&lng=${lng}`;
      return await axios.get(weatherURL).then(res => {
          return res.data;
      });
    },
    async getLocation(lat,lng) {
      let geoURL = `https://us-central1-vcrhomepage-8711c.cloudfunctions.net/getLocation?lat=${lat}&lng=${lng}`;
      return await axios.get(geoURL).then(res => {
        // we only need these two things from the api
        return {
          city : res.data.results[0].components.city,
          state : res.data.results[0].components.state_code
        }
      });
    },
    getCoords() {
      return new Promise((res, rej) => {
        return navigator.geolocation.getCurrentPosition(res,rej); 
      });
    }
  },
  async mounted() {

    // arr to hold arr promises
    let promArr = [this.getArticles()];

    // get the geolaction data
    await this.getCoords().then(
      (res) => {
        // then we can add to the promise array
        // to get other data
        console.log('on');
        this.locationOn = true;
        let lat = res.coords.latitude;
        let lng = res.coords.longitude;        
        promArr.push(this.getForecast(lat, lng));
        promArr.push(this.getLocation(lat, lng));
      }, 
      (error) => {
        console.log(error);
      }
    ).then(() => {
      // now we can resolve all promises and set values
      Promise.all(promArr).then((values) => {
        console.log('resolving');
        this.articles = values[0];

        if (this.locationOn) {
          this.dailyForecast = values[1];
          this.location = values[2];
        }
      }).then(async () => {
        // time to load the page
        // wait 500 ms so that animation works properly
        this.loaded = true;
        await new Promise(r => setTimeout(r, 1));
        this.animateHeader();
        // console.log(this.dailyForecast.daily);
      });

    });

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

h1, h2, p, a {
  text-align: center;
}

#clocks {
  margin: auto;
  text-align: center;
}

p {
  font-size: 20px;
}

</style>
