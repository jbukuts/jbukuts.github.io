<template>
    <center id="center">
        <h1>
            <mark style="font-size: 85px">404</mark><br>
            Not real
        </h1>
        <h3>
            <a href="/">Return home?</a>
        </h3>
    </center>
</template>

<script>
export default {
    name: 'NotFound'

}
</script>

<style scoped>

mark {
    color: white;
    background: rgb(0, 0, 0);
    padding-left: 7px;
    padding-right: 7px;
}

h1 {
    font-size: 46px;
}

#center {
    width: 50%;
    margin: auto;
    text-align: left;
}

a {
    text-decoration: none;
    color: black;
}


</style>