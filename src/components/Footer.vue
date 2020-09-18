<template>
    <div class="footer">
        <p>Created using VueJs</p>
    </div>
</template>

<script>
export default {
    name : 'Footer'
}
</script>

<style scoped>

.footer {
    width: 100%;
    height: 25px;
    background: rgb(0, 0, 0);
    position: fixed;
    bottom: 0px;
    line-height: 25px;
}

p {
    text-align: right;
    color: white;
    font-size: 14px;
    margin: 0px;
    margin-right: 25px;
}
</style>