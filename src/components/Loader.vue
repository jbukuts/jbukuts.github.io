<template>
    <div class="loader"></div>
</template>

<script>
export default {
    name : 'Loader'
}
</script>

<style scoped>
.loader {
    margin: auto;
    border: 10px solid rgb(255, 255, 255); /* Light grey */
    border-top: 10px solid #000000; /* Blue */
    border-radius: 50%;
    width: 100px;
    height: 100px;
    animation: spin 2s linear infinite;
    margin-top: calc(50vh - 100px);
    opacity: 1 !important;
}

@keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
}

</style>