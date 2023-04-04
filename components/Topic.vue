<template>
    <NuxtLink to="/moon">
        <div :style="{ 'background-image': `url(${source})` }">

            <h1>{{ title }}</h1>
        </div>
    </NuxtLink>
</template>

<script>
export default {
    props: ['title', 'source']
}
</script>

<style scoped>
div {
    width: 300px;
    height: 300px;
    border-radius: 50px;
    background-size: cover;
    background-position: center;
    display: flex;
    justify-content: center;
    align-items: flex-end;
}

div h1 {
    text-align: center;
    color: black;
    background-color: white;
    width: 80%;
    border-radius: 50px;
}
</style>