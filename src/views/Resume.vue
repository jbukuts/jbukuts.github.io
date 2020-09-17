<template>
  <div id="resume">

    <h2>Education</h2>
    <h3>{{resume.Education.university.toUpperCase()}} - {{resume.Education.location.toUpperCase()}}</h3>
    <p>{{resume.Education.degree}}</p>
    <p>{{resume.Education.dates}}</p>
    <p>Overall GPA : {{resume.Education.gpa}}</p>
    
    <h2>Experience</h2>
    <div class="exp" v-for="item in resume.Experience" :key="item.title">
        <h3>{{item.title.toUpperCase()}}, {{item.comp.toUpperCase()}} - {{item.dates[0].toUpperCase()}} - {{item.dates[1].toUpperCase()}}</h3>
        <p>{{item.desc}}</p>
    </div>
    
    <h2>Skills</h2>
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
    
  </div>
</template>

<script>
// @ is an alias to /src
import json from '../json/resume.json';

export default {
  name: 'Resume',
  data() {
      return {
          resume : json
      }
  },
  created() {
      console.log(this.resume);
  }

}
</script>

<style scoped>
#resume {
    text-align: left;
    width: 60%;
    margin-left: auto;
    margin-right: auto;
    overflow: scroll;
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
    color: rgb(100, 100, 100);
    font-style: italic;
    margin-top: 1px;
    margin-bottom: 0px;
}

h2 {
    margin-bottom: 5px;
}

</style>
