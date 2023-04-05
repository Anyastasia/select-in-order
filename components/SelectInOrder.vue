<template>
    <div>
        <section id="topic-menu" v-if="isMenuVisible">
            <h1>Select - in - Order</h1>
            <div class="topic-options-container">
                <Topic source="https://i.pinimg.com/736x/a8/35/99/a8359975e7d8374921dc5433af815a8c.jpg" title="Four Seasons"
                    @on-select="selectGame" />
                <Topic source="https://www.bing.com/images/blob?bcid=S9-eaSqt5HAFvw" title="Plant Cycle"
                    @on-select="selectGame" />
                <Topic
                    source="https://static.vecteezy.com/system/resources/previews/000/212/370/non_2x/moon-phase-vectors.png"
                    title="Moon Phases" @on-select="selectGame" />
            </div>
        </section>
        <section id="four-seasons" v-if="isTopicOneVisibile">
            <h1>Four season</h1>
        </section>
        <section id="plant-cycle" v-if="isTopicTwoVisibile">
            <h1>Plant Cycle</h1>
        </section>
        <section id="moon-phases" v-if="isTopicThreeVisibile">
            <h1>Moon Phases</h1>
        </section>
    </div>
</template>
  
<script>
export default {
    data() {
        return {
            isMenuVisible: true,
            isTopicOneVisibile: false,
            isTopicTwoVisibile: false,
            isTopicThreeVisibile: false
        }
    },
    methods: {
        selectGame(gameTitle) {
            console.log("hello")
            this.isMenuVisible = false;
            if (gameTitle === 'Four Seasons') {
                this.isTopicOneVisibile = true;
                this.isTopicTwoVisibile = false
                this.isTopicThreeVisibile = false
            }
            else if (gameTitle === 'Plant Cycle') {
                this.isTopicOneVisibile = false;
                this.isTopicTwoVisibile = true
                this.isTopicThreeVisibile = false
            }
            else if (gameTitle === 'Moon Phases') {
                this.isTopicOneVisibile = false;
                this.isTopicTwoVisibile = false
                this.isTopicThreeVisibile = true
            }
        }
    }
}
</script>
  
<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Changa+One&display=swap');

html {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

#topic-menu {
    width: 100vw;
    height: 100vh;
    background-image: url('https://images4.alphacoders.com/555/thumb-1920-555581.jpg');
    background-repeat: no-repeat;
    background-size: cover;
    position: relative;
}

#topic-menu h1 {
    font-family: 'Changa One';
    font-style: normal;
    font-weight: 400;
    font-size: 150px;
    line-height: 159px;
    text-align: center;
    /* color: rgb(255, 176, 72); */
    text-shadow: 0px 4px 4px rgba(42, 39, 230, 0.25);

    background: -webkit-linear-gradient(90deg, #EEF85B 5%, #ee5914 53%);
    -webkit-background-clip: text;
    -webkit-text-stroke: 15px transparent;
    color: #ffffff;
}


.topic-options-container {
    width: 100vw;
    display: flex;
    flex-direction: row;
    justify-content: space-evenly;
}
</style>
  
  