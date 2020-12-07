<template>
    <div class="weather-card">
        <div class="inner" style="text-align:left; width: 40%;">
            <p><b>{{getDay(card.dt)}}</b></p>
        </div>
        
        <div class="inner" style="width: 40%;">
            <span><img :src="`http://openweathermap.org/img/wn/${card.weather[0].icon}.png`"/></span>
        </div>

        <div class="inner" style="text-align: right; width: 10%;">
            <p class="min">{{Math.round(card.temp.min)}}°</p>
        </div>

        <div class="inner" style="text-align: right; width: 10%;">
            <p class="max">{{Math.round(card.temp.max)}}°</p>
        </div>
    </div>
    
</template>

<script>
export default {
    name : 'WeatherCard',
    data() {
        return {
            days : ['Sunday','Monday','Tuesday','Wednesday','Thursday','Friday','Saturday']
        }
    },
    props : {
        card : {type : Object}
    },
    methods : {
        getDay(dt) {
            let d = new Date(dt * 1000);
            //console.log(dt);
            //console.log(d.getFullYear());
            return this.days[d.getDay()];
        }

    }
}
</script>

<style scoped>

.weather-card {
    text-align: center;
}

img {
    filter: grayscale(1);
    vertical-align: middle;
}

span {
    display: inline-block;
    height: 100%;
    vertical-align: middle;
}

.inner {
    display: inline-block;
}

</style>