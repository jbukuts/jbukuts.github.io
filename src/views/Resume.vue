<template>
  <div id="resume" :style="$isMobile() ? 'width: 95%;' : 'width: 60%;'">

    <h2><mark>Education</mark></h2>
    <h3>{{resume.Education.university.toUpperCase()}} - {{resume.Education.location.toUpperCase()}}</h3>
    <p>{{resume.Education.degree}}</p>
    <p>{{resume.Education.dates}}</p>
    <p>Overall GPA : {{resume.Education.gpa}}</p>
    
    <h2><mark>Experience</mark></h2>
    <div class="exp" v-for="item in resume.Experience" :key="item.title">
        <h3>{{item.title.toUpperCase()}}, {{item.comp.toUpperCase()}} - {{item.dates[0].toUpperCase()}} - {{item.dates[1].toUpperCase()}}</h3>
        <p>{{item.desc}}</p>
    </div>
    
    <h2><mark>Skills</mark></h2>
    <div class="skills" v-for="item in resume.Skills" :key="item.desc">
        <p>{{item.desc}}</p>
        <ul v-if="item.list.length != 0">
            <li v-for="i in item.list" :key="i.desc">
                {{ i.desc }}
                <ul v-if="i.sublist.length != 0">
                    <li v-for="j in i.sublist" :key="j.desc">{{ j }}</li>
                </ul>
            </li>
        </ul>
    </div>

    <h2><mark>Honors</mark></h2>
    <ul>
        <li v-for="h in resume.Honors" :key="h">
            {{h}}
        </li>
    </ul>

    
  </div>
</template>

<script>
// @ is an alias to /src
import json from '../json/resume.json';
import VueMobileDetection from "vue-mobile-detection";
import Vue from 'vue';

Vue.use(VueMobileDetection);

export default {
  name: 'Resume',
  data() {
      return {
          resume : json
      }
  },
  created() {
      //console.log(this.resume);
  }

}
</script>

<style scoped>

mark {
  color: white;
  background: black;
  padding: 2.5px;
  border-radius: 0px;
}

#resume {
    text-align: left;
    margin-left: auto;
    margin-right: auto;
    overflow: hidden;
}

.exp {
    margin-bottom: 15px;
}

p {
    margin-top: 1px;
    margin-bottom: 0px;
    font-size: 16px;
}

ul {
    margin-bottom: 0px;
    margin-top: 0px;
    font-size: 16px;
}

h3 {
    font-size: 16px;
    color: rgb(65, 65, 65);
    font-style: italic;
    margin-top: 1px;
    margin-bottom: 0px;
}

h2 {
    margin-bottom: 5px;
}

</style>
