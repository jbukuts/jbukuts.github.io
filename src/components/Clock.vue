<template>
    <div class="clock">
        <canvas class="canvas" :id="name" width="200" height="200"></canvas>
        <h2>{{this.name.substring(this.name.indexOf('/')+1).replace('_'," ").toUpperCase()}}</h2>
    </div>
</template>

<script>
import moment from 'moment-timezone';

export default {
    name : 'Clock',
    props : {
        name : { type : String },
        timeZone : { type : String}
    },
    mounted() {
        console.log(this.name);
        console.log(moment.tz(this.timeZone).format("YYYY-MM-DDTHH:mm:ss"));

        let canvas = document.getElementById(this.name);
        let ctx = canvas.getContext("2d");
        let radius = canvas.height / 2;
        ctx.translate(radius, radius);
        radius = radius * 0.90;

        this.drawFace(ctx, radius);
        this.drawTime(ctx,radius);

        setInterval(async () => {
            this.drawFace(ctx, radius);
            this.drawTime(ctx,radius);
        }, 1000);

    },
    methods : {
        drawFace(ctx, radius) {
            ctx.beginPath();
            ctx.arc(0, 0, radius, 0, 2*Math.PI);
            ctx.fillStyle = 'white';
            ctx.fill();
            ctx.lineWidth = radius*0.05;
            ctx.stroke();
            ctx.beginPath();
        },
        drawTime(ctx, radius) {
            var now = new Date(moment.tz(this.timeZone).format("YYYY-MM-DDTHH:mm:ss"));
            var hour = now.getHours();
            var minute = now.getMinutes();
            var second = now.getSeconds();
            //hour
            hour=hour%12;
            hour=(hour*Math.PI/6)+
            (minute*Math.PI/(6*60))+
            (second*Math.PI/(360*60));
            this.drawHand(ctx, hour, radius*0.5, radius*0.07), 'rgba(0, 0, 0, 1)';
            //minute
            minute=(minute*Math.PI/30)+(second*Math.PI/(30*60));
            this.drawHand(ctx, minute, radius*0.8, radius*0.07, 'rgba(0, 0, 0, 1)');
            // second
            second=(second*Math.PI/30);
            this.drawHand(ctx, second, radius*0.9, radius*0.02, 'rgba(255, 0, 0, 1)');
        },
        drawHand(ctx, pos, length, width, color) {
            ctx.beginPath();
            ctx.strokeStyle = color;
            ctx.lineWidth = width;
            //ctx.lineCap = "round";
            ctx.moveTo(0,0);
            ctx.rotate(pos);
            ctx.lineTo(0, -length);
            ctx.stroke();
            ctx.rotate(-pos);
            ctx.strokeStyle = 'rgba(0, 0, 0, 1)';
        }
    }
}
</script>

<style scoped>

.clock {
    width: auto;
    margin: auto;
    margin-left: 10px;
    margin-right: 10px;
    padding: 10px;
    display: inline-block;
    background: rgb(138, 138, 138);
    border-radius: 25px 25px 0px 0px;
}

h2 {
    color: white;
    text-align: center;
    border-radius: 5px;
    background: rgb(126, 6, 6);
    width: 100%;
}

.canvas {
    padding-left: 0;
    padding-right: 0;
    margin-left: auto;
    margin-right: auto;
    display: block;
}

</style>