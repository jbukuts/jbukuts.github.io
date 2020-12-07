<template>
    <div class="article">
        <h3 style="text-align: right">
        <mark><a :href="art.url">{{art.title.substring(0,art.title.indexOf(" - "))}}</a></mark>
        </h3>

        <p v-if="art.author != null && art.author != ''" style="text-align: left; font-size: 12px; margin-bottom: 0px">
            By <b>{{art.author.toUpperCase()}}</b>
        </p>

        <p style="text-align: left; font-size: 12px; margin-top: 0px">
            Published at {{dateToString(art.publishedAt)}}
        </p>

        <img :src='art.urlToImage' style="border-radius: 10px; filter: grayscale(100%); max-height: 140px; max-width: 90%" />
        <p style="font-size: 14px; text-align: center; padding: 0px 10px 0px 10px">{{art.description}}</p>
    </div>
    
</template>

<script>
export default {
    name : 'Article',
    props : {
        art : { type : Object }
    },
    methods : {
        dateToString(d) {
            var date = new Date(d);
            return `${date.toLocaleString('default', { month: 'short' })} ${date.getDate()}, ${date.getFullYear()} `;
        },
    }
}
</script>


<style scoped>

mark {
  color: white;
  background: rgb(0, 0, 0);
  padding-left: 7px;
  padding-right: 7px;
}

mark:hover {
  cursor: pointer;
  text-decoration: underline;
}

a {
  text-decoration: none;
  color: white;
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
  /* Declaring our shadow color inherit from the parent (button) */
  background: linear-gradient(270deg, #ff0000, #d58516, #aeb728, #00813e, #003681, #b757c1);
  background-size: 1200% 1200%;
  -webkit-animation: AnimationName 20s ease infinite;
  animation: AnimationName 20s ease infinite;
}


.article {
  border: 1px solid black;
  margin: auto;
  padding: 0px;
  text-align: center;
  padding: 10px;
  height: 480px;
  background: white;
  position: relative;
  margin-right: 10px;
  margin-left: 10px;
  margin-bottom: 10px;
}

</style>
