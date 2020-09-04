<template>
    <div class="projects">

        <div class="exp" v-for="item in this.projects" :key="item.id">
            <a :href="item.url">{{item.name}}</a>
            <p>{{item.description}}</p>

            <div class="languange-bar">{{getLangs(item.languages_url)}}</div>
            
        </div>



    </div>
</template>

<script>
import axios from 'axios';

export default {
    name : 'Projects',
    data() {
        return {
            projects : []
        }
    },
    async mounted() {
        this.projects = await this.getRepos();
        console.log(this.projects);
    },
    methods : {
        async getRepos() {

            let repos;
            await axios.get('https://api.github.com/users/jbukuts/repos').then(response => {
                console.log(response.data);
                repos = response.data;
            });
            
            return repos;
        },
        getLangs(url) {
            let langs;
            return axios.get(url).then(response => {
                langs = response.data;
                for (var key of Object.keys(langs)) {
                    console.log(key + " -> " + langs[key])
                } 
                return "";
            });
        }
    }
}
</script>

<style scoped>

</style>