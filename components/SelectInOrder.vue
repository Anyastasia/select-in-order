<template>
    <div>
        <section id="topic-menu" v-if="isMenuVisible">
            <h1>Select - in - Order</h1>
            <div class="topic-options-container">
                <!-- <Topic source="https://i.pinimg.com/736x/a8/35/99/a8359975e7d8374921dc5433af815a8c.jpg" title="Four Seasons"
                    @on-select="selectGame" /> -->
                <Topic source="https://www.bing.com/images/blob?bcid=S9-eaSqt5HAFvw" title="Plant Cycle"
                    @on-select="selectGame" />
                <Topic
                    source="https://static.vecteezy.com/system/resources/previews/000/212/370/non_2x/moon-phase-vectors.png"
                    title="Moon Phases" @on-select="selectGame" />
            </div>
        </section>
        <section id="four-seasons" class="game-section" v-if="isTopicOneVisibile">
            <h1>Four season</h1>
        </section>
        <section id="plant-cycle" class="game-section" v-if="isTopicTwoVisibile">
            <div class="game-details">
                <h1 @click="quitGame">
                    &lt; Back</h1>
                <h1>Timer: 30</h1>
            </div>
            <h1 class="game-title">{{ activeGameTitle }}</h1>
            <div class="grid-container">

            </div>
        </section>
        <section id="moon-phases" class="game-section" v-if="isTopicThreeVisibile">
            <div class="game-details">
                <h1 @click="quitGame">
                    &lt; Back</h1>
                <h1>Timer: 30</h1>
            </div>
            <h1 class="game-title">{{ activeGameTitle }}</h1>
            <div class="grid-container">
                <Grid v-for="(phase, index) in moonPhase" :key="index" :correctSequence="phase.sequence"
                    :source="phase.source" :currentSequence="currentSequence" @add-sequence="addSequence"
                    @subtract-sequence="subtractSequence" />
            </div>
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
            isTopicThreeVisibile: false,
            currentSequence: 1,
            sequenceStack: [],
            activeGameTitle: "",
            moonPhase: [
                { sequence: 1, source: "https://www.astro-seek.com/seek-images/fb/seek_fb_moon_new_1200.jpg" },
                { sequence: 2, source: "https://i.redd.it/8o9j69nh19a11.jpg" },
                { sequence: 3, source: "https://images.fineartamerica.com/images-medium-large-5/first-quarter-moon-nasas-scientific-visualization-studioscience-photo-library.jpg" },
                { sequence: 4, source: "https://live.staticflickr.com/3874/19223685325_0b9a3b0ab9.jpg" },
                { sequence: 5, source: "https://i.pinimg.com/736x/eb/49/cc/eb49cc84a06c03d8de19d45e3ab58509--earth-space-new-moon.jpg" },
                { sequence: 6, source: "https://th.bing.com/th/id/OIP.1JqQcG3Yz93qcvikjUiLPQHaHa?pid=ImgDet&rs=1" },
                { sequence: 7, source: "https://live.staticflickr.com/3879/14757366867_102a4c3cd3.jpg" },
                { sequence: 8, source: "https://imgc.artprintimages.com/img/print/waning-crescent-moon_u-l-pzhkty0.jpg?h=550&p=0&w=550&background=fbfbfb" },
               
            ]
        }
    },
    methods: {
        selectGame(gameTitle) {
            console.log("hello")
            this.isMenuVisible = false;
            this.activeGameTitle = gameTitle;
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
        },
        quitGame() {
            this.isMenuVisible = true;
            this.isTopicOneVisibile = false;
            this.isTopicTwoVisibile = false
            this.isTopicThreeVisibile = false
            this.currentSequence = 1;
            this.sequenceStack = []
        },
        addSequence(element) {
            const closestGrid = element.closest(".grid");
            this.sequenceStack.push(closestGrid);
            this.currentSequence += 1;
            console.log(this.sequenceStack);
        },
        subtractSequence(element) {
            const closestGrid = element.closest(".grid");
            this.sequenceStack = this.sequenceStack.filter(grid => grid !== closestGrid);
            this.currentSequence -= 1;
            console.log(this.sequenceStack);
        }
    },
    watch: {
        currentSequence(newValue, prevValue) {
            let sequenceCount = 1;
            this.sequenceStack.forEach(grid => {
                const badge = grid.children[0];
                const badgeText = badge.children[0];
                badgeText.textContent = sequenceCount;
                sequenceCount++;
            });
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

.game-section {
    width: 100vw;
    height: 100vh;
    padding: 30px 50px;
}

.game-details {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
}

.game-details h1 {
    padding: 5px;
    font-family: 'Changa One';
    font-style: normal;
    font-weight: 400;
    font-size: 50px;
    line-height: 68px;
    color: #ffffff;
}

.game-details h1:first-of-type {
    cursor: pointer;
    transition: all 250ms ease-in-out;
}

.game-details h1:first-of-type:hover {
    transform: scale(1.1);
}

.game-title {
    padding: 5px;
    font-family: 'Changa One';
    font-style: normal;
    font-weight: 400;
    font-size: 5rem;
    line-height: 68px;
    color: #ffffff;
    text-align: center;
}

.grid-container {
    /* display: grid;
    grid-template-columns: repeat(4, 200px);
    grid-auto-rows: auto;
    row-gap: 50px;
    column-gap: 50px;
    justify-content: center;
    align-items: center; */
    width: 60vw;
    height: 60vh;
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;

    justify-content: space-evenly;
    align-items: center;
    margin: auto;
}

#moon-phases {
    background-image: url('https://i.pinimg.com/originals/1c/6e/f2/1c6ef21ebd4054a417b6127bdbece20b.jpg');
    background-size: cover;
    background-repeat: no-repeat;
}

#moon-phases h1 {
    text-shadow: 0px 4px 4px rgba(42, 39, 230, 0.25);
    background: -webkit-linear-gradient(90deg, #0F79A3 5%, #3a3e47 53%);
    -webkit-background-clip: text;
    -webkit-text-stroke: 10px transparent;
}

#plant-cycle {
    background-image: url('https://i.pinimg.com/originals/ab/41/a0/ab41a0edeaf154cf9e82715f52511dac.png');
    background-size: cover;
    background-repeat: no-repeat;
}

#plant-cycle h1 {
    text-shadow: 0px 4px 4px rgba(42, 39, 230, 0.25);
    background: -webkit-linear-gradient(90deg, #525252 5%, #1E63E9 53%);
    -webkit-background-clip: text;
    -webkit-text-stroke: 10px transparent;
}
</style>
  
  