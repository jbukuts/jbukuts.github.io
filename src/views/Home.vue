<template>
  <div class="home">
    <h1>Welcome to my Github Site</h1>

    <!--<div id="clocks" >
      <Clock v-for="c in this.times" :key="c" :name='c' :timeZone='c' />
    </div>-->

    <h1>TOP ARTICLES TODAY</h1>
    <hr style="margin-bottom: 30px">

    <div class="article-container" v-if="articles.length > 0">
      <div class="article" :class="{side : (index != currArt)}" 
        v-for="index in [(currArt-1 < 0) ? this.articles.length-1 : currArt-1, currArt, (currArt+1 > this.articles.length-1) ? 0 : currArt+1]" :key=index  v-on:click="moveCurr(index)">
        <h3>{{articles[index].title}}</h3>
        <p v-if="articles[index].author != null" style="text-align: left; font-size: 12px; margin-bottom: 0px">By <b>{{articles[index].author.toUpperCase()}}</b></p>
        <p style="text-align: left; font-size: 12px; margin-top: 0px">Published at {{dateToString(articles[index].publishedAt)}}</p>
        <img :src='articles[index].urlToImage' style="width: 90%; border-radius: 10px; filter: grayscale(100%)" />
        <p v-if="index == currArt" style="font-size: 14px">{{articles[index].description}}</p>
        <a v-if="index == currArt" :href="articles[index].url" style="color: black; font-weight: bold; font-size: 14px">CLICK TO READ FULL ARTICLE</a>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
//import Clock from "../components/Clock.vue";
import axios from 'axios';


export default {
  name: 'Home',
  components : {
    //Clock
  },
  data() {
    return {
      times : [ "Asia/Tokyo", "America/Los_Angeles","America/New_York" ] ,
      articles : [],
      currArt : 1
    }
  },
  methods : {
    moveCurr(index) {
      this.currArt = index;
    },
    dateToString(d) {
      var date = new Date(d);
      return `${date.toLocaleString('default', { month: 'short' })} ${date.getDate()}, ${date.getFullYear()} `;
    }
  },
  async mounted() {
    this.articles = await axios.get('https://us-central1-vcrhomepage-8711c.cloudfunctions.net/getTopHeadlines').then((d) => {
      console.log(d.data.articles.map(x => x.source.name));
      return d.data.articles;
    });

    console.log(this.articles);
  }
}
</script>

<style scoped>

.article-container {
  width: 100%;
  margin: auto;
}

.side:hover {
  filter: blur(0px);
  transform: scale(.9);
  cursor: pointer;
}

.article {
  width: calc(33% - 20px);
  float: left;
  border: 1px solid black;
  border-radius: 10px;
  margin: auto;
  padding: 0px;
  text-align: center;
  padding: 10px;
  height: auto;
  background: white;
  position: relative;
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
  border-radius: 10px;
  /* Declaring our shadow color inherit from the parent (button) */
  background: linear-gradient(270deg, #ff0000, #d58516, #aeb728, #00813e, #003681, #b757c1);
  background-size: 1200% 1200%;
  -webkit-animation: AnimationName 20s ease infinite;
  animation: AnimationName 20s ease infinite;
}

.side::after {
  opacity: 0;
}

.side {
  transform: scale(.7);
  transition: filter .5s, transform .25s;
  filter: blur(4px);
  height: auto;
}

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
