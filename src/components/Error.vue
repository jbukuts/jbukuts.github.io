<template>
    <div>
        <mark>There was an error</mark>
        <h2>{{info}}</h2>
    </div>
</template>

<script>
export default {
    props: ['info'],
    name: 'Error'
}
</script>

<style scoped>

div {
    width: 90%;
    margin: auto;
    margin-top: 50px;
    text-align: center;
}

mark {
    background: black;
    color: white;
    font-size: 48px;
    padding: 7px;
}

</style>