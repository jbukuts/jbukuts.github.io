<template>
    <div class="projects" >

        <Loader v-if='!this.loaded && !this.failed'></Loader>

        <Error v-if="this.failed" info="Too many request made to API"></Error>

        <div v-if='this.loaded' :style="$isMobile() ? 'width: 90%;' : 'width: 60%;'" style="margin: auto">
            <h1>My projects</h1>
            
            <div style="display: none">
                <h3>Color Key</h3>
                <div v-for="lang in Object.keys(this.langColors)" :key="lang" style="display: flex; margin-bottom: 5px">
                    <div :style="`width: 20px; height: 20px; border-radius: 50%; background: ${langColors[lang]}`"></div>
                    <p style="margin: 0px; margin-left: 7px">{{lang}}</p>
                </div>
            </div>

            <div class="proj" v-for="item in this.projects" :key="item.id">
                <a :href="item.html_url">{{item.name}}</a>
                <p>{{item.description}}</p>

                <!--<div id="languange-bar" :style="createBarStyle(item.langs_data)"></div>-->
                <CodeBar v-if='loaded' :list=item.langs_data :colors=langColors />
            </div>
        </div>
    </div>
</template>

<script>
import Loader from '../components/Loader.vue';
import CodeBar from '../components/CodeBar.vue';
import Error from '../components/Error.vue';
import axios from 'axios';
import Vue from 'vue';
import VueMobileDetection from "vue-mobile-detection";

Vue.use(VueMobileDetection);

export default {
    name : 'Projects',
    components : {
        Loader,
        CodeBar,
        Error
    },
    data() {
        return {
            projects : [],
            allLangs : [],
            langColors : {},
            loaded : false,
            failed: false
        }
    },
    async mounted() {

        this.projects = await this.getRepos();   
        
        if (this.projects == false) {
            this.failed = true;
            return;
        }
        
        const reducer= (acc, curr) => acc.concat((Object.keys(curr.langs_data)));
        this.allLangs = [...new Set(this.projects.reduce(reducer, []).filter(x => x != 'total'))];
   
        //console.log(this.allLangs);
        this.langColors = this.generateColors(this.allLangs);
        //console.log(this.langColors);
        this.loaded = true;
    },
    methods : {
        generateColors(langList) {
            // based on length of how many languanges we can generate a color list

            let colorInc = 340 / langList.length;

            let temp = {};

            for (let i = 0; i< langList.length; ++i) {
                temp[langList[i]] = `hsl(${i * colorInc} , 90%, 35%)`;
            }

            return temp;
        },
        createBarStyle(list) {

            let style = "height: 15px; border-radius: 10px ;background:";
            let lastPerc = 0;

            function Perc(percent, lang) {
                this.percent = percent;
                this.lang = lang;
            }

            let percentArr = Object.keys(list).filter(key => key != 'total').map((x) => new Perc((list[x]/list.total) * 100, x));
            //console.log(percentArr);

            if (percentArr.length == 1)
                return style + `${this.langColors[percentArr[0].lang]};`;
            
            style += "linear-gradient(90deg,";

            for (let i=0; i < percentArr.length-1; ++i) {
                style += `${this.langColors[percentArr[i].lang]} ${percentArr[i].percent + lastPerc}%,`;
                style += `${this.langColors[percentArr[i+1].lang]} ${percentArr[i].percent + lastPerc}%,`;
                lastPerc = percentArr[i].percent;
            } 

            style = style.substring(0, style.lastIndexOf(','));
            style += ");";

            //console.log(style);
            return style;
        },
        async getRepos() {
            return await axios.get('https://api.github.com/users/jbukuts/repos').then(async response => {

                for (let i = 0; i<response.data.length; i++) {
                    response.data[i]['langs_data'] = await this.getLangs(response.data[i].languages_url);
                    // console.log(this.projects[i]['langs_data']);
                }

                return response.data;
            }).catch( () => {
                //console.log(err);
                return false;
            });
        },
        async getLangs(url) {
            return await axios.get(url).then(response => {
                let total = 0;
                for (var key of Object.keys(response.data)) {
                    total += response.data[key];
                } 

                response.data.total = total;
                return response.data;
            });
        }
    }
}
</script>

<style scoped>


.projects {
    height: 100%;
    overflow: scroll;
    margin-right: auto;
    margin-left: auto;
}

.proj {
    border: 1px solid black;
    border-radius: 0px;
    padding: 15px;
    padding-bottom: 30px;
    margin: auto;
    margin-bottom: 20px;
    box-shadow: -5px 5px rgb(0, 0, 0);
}


.proj a {
    text-decoration: none;
    color: black;
    font-weight: bold;
    font-size: 20px;
}

.proj a:hover {
    text-decoration: underline;
}


</style>