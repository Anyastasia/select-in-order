<template>
    <div>
        <div class="topic-container">
            <div class="topic-image">
                <img :src="source" alt="">
            </div>
        </div>
        <div class="button" @click="onClick">
            <h1>{{ title }}</h1>
        </div>
    </div>
</template>

<script>
export default {
    props: ['source', 'title'],
    emits: ['on-select'],
    methods: {
        onClick() {
            console.log("hello from topic")
            this.$emit('on-select', this.title);
        }
    }
}
</script>

<style scoped>
.topic-container {
    width: 365px;
    height: 295px;
    background: linear-gradient(180deg, #E1E445 0%, #E44545 100%);
    box-shadow: 0px 0px 4px 12px rgba(0, 0, 0, 0.25);
    border-radius: 50px;
    display: flex;
    justify-content: center;
    align-items: center;
    margin-bottom: 50px;
}

.topic-image {
    width: 90%;
    height: 90%;
    border-radius: 50px;
    object-fit: cover;
    background-color: white;
    overflow: hidden;
}

.topic-image img {
    width: 100%;
    height: 100%;
}

.button {
    width: 365px;
    height: 93px;
    background: linear-gradient(180deg, #22DC4B 0%, #DAC613 100%);
    box-shadow: 0px 0px 4px 12px rgba(0, 0, 0, 0.25);
    border-radius: 20px;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    transition: all 250ms ease-in-out;
}

.button:hover {
    transform: translateY(-10px);
}

.button h1 {
    font-family: 'Changa One';
    font-style: normal;
    font-weight: 400;
    font-size: 48px;
    line-height: 51px;
    color: #FFFFFF;
    text-shadow: 0px 4px 4px #000000;
    margin: 0px;
}
</style>