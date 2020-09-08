<template>
    <div class="projects" >

        <Loader v-if='!this.loaded'></Loader>

        <div v-if='this.loaded' >
            <h1>My Projects</h1>
            <div style="position: fixed; top: 5%; right: 10%; z-index: 1">
                <h3>Color Key</h3>
                <div v-for="lang in Object.keys(this.langColors)" :key="lang" style="display: flex; margin-bottom: 5px">
                    <div :style="`width: 20px; height: 20px; border-radius: 50%; background: ${langColors[lang]}`"></div>
                    <p style="margin: 0px; margin-left: 7px">{{lang}}</p>
                </div>
            </div>

            <div class="proj" v-for="item in this.projects" :key="item.id">
                <a :href="item.html_url">{{item.name}}</a>
                <p>{{item.description}}</p>

                <div id="languange-bar" :style="createBarStyle(item.langs_data)"></div>
            </div>
        </div>

    </div>
</template>

<script>
import Loader from '../components/Loader.vue';
import axios from 'axios';

export default {
    name : 'Projects',
    components : {
        Loader
    },
    data() {
        return {
            projects : [],
            allLangs : [],
            langColors : {},
            loaded : false
        }
    },
    async mounted() {
        this.projects = await this.getRepos();        
        const reducer= (acc, curr) => acc.concat((Object.keys(curr.langs_data)));
        this.allLangs = [...new Set(this.projects.reduce(reducer, []).filter(x => x != 'total'))];
   
        console.log(this.allLangs);
        this.generateColors(this.allLangs);
        console.log(this.langColors);
        this.loaded = true;
    },
    methods : {
        generateColors(langList) {
            // based on length of how many languanges we can generate a color list

            let colorInc = 340 / langList.length;

            for (let i = 0; i< langList.length; ++i) {
                this.langColors[langList[i]] = `hsl(${i * colorInc} , 100%, 25%)`;
            }
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
    width: 55%;
    height: 100%;
    overflow: scroll;
    margin-right: auto;
    margin-left: auto;
    margin-top: 15px;
}

.proj {
    border: 1px solid black;
    border-radius: 0px;
    padding: 15px;
    margin: auto;
    margin-bottom: 20px;
    box-shadow: 5px 5px rgba(37, 37, 37, .3);
    width: 90%;
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