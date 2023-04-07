<template>
    <div :id="correctSequence" class="grid" @click="onClick">
        <div class="badge" v-show="isBadgeDisplayed">
            <h1>{{ sequence }}</h1>
        </div>
        <div class="image-container">
            <img :src="source" alt="">
        </div>
    </div>
</template>

<script>
export default {
    props: ['source', 'correctSequence', 'currentSequence'],
    emits: ['add-sequence', 'subtract-sequence'],
    data() {
        return {
            isBadgeDisplayed: false,
            sequence: 0
        }
    },
    methods: {
        onClick(event) {
            this.sequence = this.currentSequence;
            if (!this.isBadgeDisplayed) {
                this.$emit('add-sequence', event.target);
            }
            else {
                this.$emit('subtract-sequence', event.target);
            }
            this.isBadgeDisplayed = !this.isBadgeDisplayed;
        }
    }
}
</script>

<style scoped>
.grid {
    width: 200px;
    height: 200px;
    background: linear-gradient(180deg, #1E63E9 0%, #1E1E1E 100%);
    box-shadow: 0px 0px 4px 12px rgba(0, 0, 0, 0.25);
    border-radius: 50px;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    position: relative;
    transition: all 250ms ease-in-out;
}

.grid:hover {
    transform: scale(1.05);
}

.grid:active {
    transform: scale(.9);
}

.badge {
    position: absolute;
    top: -10px;
    right: -10px;
    width: 50px;
    height: 50px;
    background: linear-gradient(180deg, #22DC4B 0%, #13dad0 100%);
    box-shadow: 0px 0px 4px 12px rgba(0, 0, 0, 0.25);
    border-radius: 10px;
    display: flex;
    justify-content: center;
    align-items: center;
}

.badge h1{
    font-family: 'Changa One';
    font-size: 2rem;
    color: white;
    text-shadow: 0px 4px 4px rgba(42, 39, 230, 0.25);
    background: -webkit-linear-gradient(90deg, #d4bf00 5%, #c93611 53%);
    -webkit-background-clip: text;
    -webkit-text-stroke: 5px transparent;
    margin: 0;
}

.image-container {
    width: 90%;
    height: 90%;
    border-radius: 50px;
    object-fit: cover;
    overflow: hidden;
}

.image-container img {
    width: 100%;
    height: 100%;
}
</style>