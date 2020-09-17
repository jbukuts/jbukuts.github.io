<template>
    <div class="codebar">
        <div class="section" v-for="b in Object.keys(list).filter(k => k != 'total')" 
            :key=b 
            :style='{background : colors[b], width : `${(list[b]/list.total)*100}%` }' 
            v-tooltip.bottom="{ content : b, offset : 4, autoHide : true }">
        </div>

    </div>
</template>

<script>
import Vue from 'vue';
import VTooltip from 'v-tooltip';

Vue.use(VTooltip);

export default {
    name : 'CodeBar',
    components : {
    },
    props : {
        list : { type : Object },
        colors : {type : Object}
    },
    data() {
        return {
        }
    },
    mounted() {
        
    },
    methods : {
    }
}
</script>

<style scoped>

.codebar {
    width: 100%;
    height: 10px;
    border-radius: 5px;
}

.section {
    height: 10px;
    display: inline-block;
}

</style>