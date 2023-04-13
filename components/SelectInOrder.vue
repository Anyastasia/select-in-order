<template>
    <div>
        <Modal ref="gameOverModal" theme="error" message="Game Over" @quit="quitGame" @restart="restartGame" />
        <Modal ref="timeOutModal" theme="error" message="Ran out of time!" @quit="quitGame" @restart="restartGame" />
        <Modal ref="successModal" theme="success" message="Noice" @quit="quitGame" @restart="restartGame" />

        <section id="topic-menu" v-if="isMenuVisible">
            <h1>Select - in - Order</h1>
            <div class="topic-options-container">
                <Topic :source="thumbnail" :title="title" @on-select="selectGame" v-if="isValidSequence" />
                <Topic source="https://www.bing.com/images/blob?bcid=S9-eaSqt5HAFvw" title="Plant Cycle"
                    @on-select="selectGame" v-if="!hideOptions" />
                <Topic
                    source="https://static.vecteezy.com/system/resources/previews/000/212/370/non_2x/moon-phase-vectors.png"
                    title="Moon Phases" @on-select="selectGame" v-if="!hideOptions" />
            </div>
        </section>
        <section id="custom-game" class="game-section" v-if="isTopicOneVisibile">
            <div class="game-details">
                <h1 @click="quitGame">
                    &lt; Back</h1>
                <div class="life-container">
                    <i v-if="currentLife >= 1" class="bi bi-heart-fill"></i>
                    <i v-else class="bi bi-heartbreak"></i>
                    <i v-if="currentLife >= 2" class="bi bi-heart-fill"></i>
                    <i v-else class="bi bi-heartbreak"></i>
                    <i v-if="currentLife === 3" class="bi bi-heart-fill"></i>
                    <i v-else class="bi bi-heartbreak"></i>
                </div>
                <h1>Timer: {{ timerIsoString }}</h1>
            </div>
            <h1 class="game-title">{{ activeGameTitle }}</h1>
            <div class="grid-container">
                <Grid v-for="(phase, index) in newPropSequence" :key="index" :correctSequence="phase.sequence"
                    :source="phase.source" :currentSequence="currentSequence" @add-sequence="addSequence"
                    @subtract-sequence="subtractSequence" />
            </div>
        </section>
        <section id="plant-cycle" class="game-section" v-if="isTopicTwoVisibile">
            <div class="game-details">
                <h1 @click="quitGame">
                    &lt; Back</h1>
                <div class="life-container">
                    <i v-if="currentLife >= 1" class="bi bi-heart-fill"></i>
                    <i v-else class="bi bi-heartbreak"></i>
                    <i v-if="currentLife >= 2" class="bi bi-heart-fill"></i>
                    <i v-else class="bi bi-heartbreak"></i>
                    <i v-if="currentLife === 3" class="bi bi-heart-fill"></i>
                    <i v-else class="bi bi-heartbreak"></i>
                </div>
                <h1>Timer: {{ timerIsoString }}</h1>
            </div>
            <h1 class="game-title">{{ activeGameTitle }}</h1>
            <div class="grid-container">
                <Grid v-for="(phase, index) in plantCycle" :key="index" :correctSequence="phase.sequence"
                    :source="phase.source" :currentSequence="currentSequence" @add-sequence="addSequence"
                    @subtract-sequence="subtractSequence" />
            </div>
        </section>
        <section id="moon-phases" class="game-section" v-if="isTopicThreeVisibile">
            <div class="game-details">
                <h1 @click="quitGame">
                    &lt; Back</h1>
                <div class="life-container">
                    <i v-if="currentLife >= 1" class="bi bi-heart-fill"></i>
                    <i v-else class="bi bi-heartbreak"></i>
                    <i v-if="currentLife >= 2" class="bi bi-heart-fill"></i>
                    <i v-else class="bi bi-heartbreak"></i>
                    <i v-if="currentLife === 3" class="bi bi-heart-fill"></i>
                    <i v-else class="bi bi-heartbreak"></i>
                </div>
                <h1>Timer: {{ timerIsoString }}</h1>
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
    props: {
        thumbnail: {
            type: String,
            default(rawProps) {
                return "https://i.pinimg.com/736x/a8/35/99/a8359975e7d8374921dc5433af815a8c.jpg"
            }
        },
        title: {
            type: String,
            default(rawProps) {
                return "Custom game"
            }
        },
        sequence: {
            type: Array,
            default(rawProps) {
                return [];
            },
        },
        hideOptions: Boolean
    },
    data() {
        return {
            isMenuVisible: true,
            isTopicOneVisibile: false,
            isTopicTwoVisibile: false,
            isTopicThreeVisibile: false,
            newPropSequence: [],
            currentLife: 3,
            currentSequence: 1,
            sequenceStack: [],
            userSequence: [],
            gridCount: 0,
            isCorrectSequence: true,
            timerCount: 30,
            timeInterval: null,
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

            ],
            plantCycle: [
                { sequence: 1, source: "https://th.bing.com/th/id/OIP.NNZ0j0Po9RVSr0OXoVQV1wHaE7?w=277&h=185&c=7&r=0&o=5&pid=1.7" },
                { sequence: 2, source: "https://live.staticflickr.com/3347/5816757734_daa9307ba7_b.jpg" },
                { sequence: 3, source: "https://c2.staticflickr.com/2/1226/627779067_dc5049fee9_b.jpg" },
                { sequence: 4, source: "https://upload.wikimedia.org/wikipedia/commons/thumb/a/a9/A_sunflower.jpg/360px-A_sunflower.jpg" }
            ]
        }
    },
    methods: {
        selectGame(gameTitle) {
            this.isMenuVisible = false;
            this.activeGameTitle = gameTitle;
            if (gameTitle === this.title) {
                this.isTopicOneVisibile = true;
                this.isTopicTwoVisibile = false
                this.isTopicThreeVisibile = false
                this.newPropSequence = this.shuffle(this.newPropSequence)
            }
            else if (gameTitle === 'Plant Cycle') {
                this.isTopicOneVisibile = false;
                this.isTopicTwoVisibile = true
                this.isTopicThreeVisibile = false
                this.plantCycle = this.shuffle(this.plantCycle)
            }
            else if (gameTitle === 'Moon Phases') {
                this.isTopicOneVisibile = false;
                this.isTopicTwoVisibile = false
                this.isTopicThreeVisibile = true
                this.moonPhase = this.shuffle(this.moonPhase);
            }

            this.startInterval();
        },
        quitGame() {
            this.isMenuVisible = true;
            this.isTopicOneVisibile = false;
            this.isTopicTwoVisibile = false
            this.isTopicThreeVisibile = false
            this.currentSequence = 1;
            this.sequenceStack = []
            this.isCorrectSequence = true
            this.clearTimer()
            this.resetTimer()
            this.currentLife = 3
            this.userSequence = []
        },
        restartGame() {
            this.sequenceStack.forEach(value => value.click())
            this.sequenceStack = []
            this.isCorrectSequence = true
            this.clearTimer()
            this.resetTimer()
            this.currentLife = 3
            this.userSequence = []
            this.startInterval();
        },
        addSequence(element) {
            const closestGrid = element.closest(".grid");
            this.sequenceStack.push(closestGrid);
            this.currentSequence += 1;
            // console.log(this.sequenceStack);
        },
        subtractSequence(element) {
            const closestGrid = element.closest(".grid");
            this.sequenceStack = this.sequenceStack.filter(grid => grid !== closestGrid);
            this.currentSequence -= 1;
            // console.log(this.sequenceStack);
        },
        shuffle(a) {
            for (let i = a.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [a[i], a[j]] = [a[j], a[i]];
            }
            return a;
        },
        startInterval() {
            this.timeInterval = setInterval(() => {
                this.timerCount--;
                // console.log(this.timerCount)

                if (this.timerCount === 0) {
                    // alert('ran out of time!');
                    this.$refs.timeOutModal.$el.showModal()
                    clearInterval(this.timeInterval);
                }
            }, 1000);
        },
        clearTimer() {
            clearInterval(this.timeInterval);
            this.timeInterval = null;
        },
        resetTimer() {
            this.timerCount = 30;
        },
        generateValidSequence() {
            this.newPropSequence = this.sequence.map((source, index) => {
                return { sequence: index + 1, source }
            })
            console.log(this.newPropSequence)
        }
    },
    watch: {
        currentSequence(newValue, prevValue) {
            let sequenceCount = 1;
            this.sequenceStack.forEach(grid => {
                // this.isCorrectSequence = true;
                const badge = grid.children[0];
                const badgeText = badge.children[0];
                badgeText.textContent = sequenceCount;

                //check if sequence is still correct
                if (grid.id != badgeText.textContent) this.userSequence.push(false);
                else this.userSequence.push(true);
                this.isCorrectSequence = this.userSequence.every((currentValue) => currentValue)

                sequenceCount++;
                // console.log(this.isCorrectSequence)
                // console.log(this.userSequence)
            });
            this.userSequence = []
        },
    },
    updated() {
        if (!this.isMenuVisible) {
            this.gridCount = document.querySelector(".grid-container").children.length;


            if (this.gridCount === this.sequenceStack.length) {

                if (this.isCorrectSequence) {
                    // alert('the sequence is right mlem');
                    this.$refs.successModal.$el.showModal()
                    this.clearTimer()
                }
                else {
                    this.currentLife--;
                    console.log(this.currentLife)
                    if (this.currentLife === 0) {
                        // alert('bro are you that dumb?')
                        this.$refs.gameOverModal.$el.showModal()
                        this.clearTimer()
                    }
                    this.sequenceStack[this.sequenceStack.length - 1].click()
                    // this.sequenceStack.pop()
                }
            }
        }
    },
    computed: {
        isValidSequence() {
            return this.sequence.length >= 2 && this.sequence.length <= 8;
        },
        timerIsoString() {
            const date = new Date(0);
            date.setSeconds(this.timerCount);
            return date.toISOString().substring(17, 19);
        }
    },
    mounted() {
        if (this.sequence.length >= 2 && this.sequence.length <= 8) {
            this.generateValidSequence();
        }
    }
}
</script>
  
<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Changa+One&display=swap');
@import url("https://cdn.jsdelivr.net/npm/bootstrap-icons@1.10.4/font/bootstrap-icons.css");

html {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    overflow: hidden;
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
    flex-wrap: wrap;
}

.game-section {
    width: 100vw;
    height: 100vh;
    padding: 30px 50px;
    background-position: center;
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

.life-container {
    height: 100%;
    width: 200px;
    display: flex;
    flex-direction: row;
    justify-content: space-evenly;
    align-items: center;
    /* background: rgba(236, 203, 17, 0.507);
    box-shadow: 0px 0px 4px 12px rgba(0, 0, 0, 0.048);
    border-radius: 20px; */
}

.life-container i {
    font-size: 45px;
    color: #ee4116;
    /* text-shadow: #3a3e47; */
    filter: drop-shadow(0 0 0.4rem crimson);
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
    width: 50vw;
    height: auto;
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

#custom-game {
    background-image: url('https://images3.alphacoders.com/918/918325.jpg');
    background-size: cover;
    background-repeat: no-repeat;
}

#custom-game h1 {
    text-shadow: 0px 4px 4px rgba(42, 39, 230, 0.25);
    background: -webkit-linear-gradient(90deg, #525252 5%, #1E63E9 53%);
    -webkit-background-clip: text;
    -webkit-text-stroke: 10px transparent;
}

.bi-heartbreak {
    animation: shake 0.5s;
}

@keyframes shake {
  0% { transform: translate(1px, 1px) rotate(0deg); }
  10% { transform: translate(-1px, -2px) rotate(-1deg); }
  20% { transform: translate(-3px, 0px) rotate(1deg); }
  30% { transform: translate(3px, 2px) rotate(0deg); }
  40% { transform: translate(1px, -1px) rotate(1deg); }
  50% { transform: translate(-1px, 2px) rotate(-1deg); }
  60% { transform: translate(-3px, 1px) rotate(0deg); }
  70% { transform: translate(3px, 1px) rotate(-1deg); }
  80% { transform: translate(-1px, -1px) rotate(1deg); }
  90% { transform: translate(1px, 2px) rotate(0deg); }
  100% { transform: translate(1px, -2px) rotate(-1deg); }
}


@media only screen and (max-width: 1440px) {
    #topic-menu h1 {
        font-size: 100px;
    }

    .grid-container {
        width: 60vw;
    }
}

@media only screen and (max-width: 1024px) {
    .grid-container {
        width: 90%;
    }
}

@media only screen and (max-width: 768px) {

    #topic-menu h1 {
        font-size: 80px;
        margin-bottom: 0px;
    }

    .topic-options-container {
        padding: 10px;
    }

    .game-title {
        font-size: 4rem;
    }

    .game-details h1 {
        font-size: 40px;
    }

    .grid-container {
        width: 80%;
        /* height: 40vh; */
    }
}

@media only screen and (max-width: 425px) {

    #topic-menu h1 {
        font-size: 50px;
        margin-bottom: 0px;
    }

    .topic-options-container {
        height: 75vh !important;
        overflow: scroll;
    }

    .game-section {
        padding: 20px;
    }

    .game-title {
        font-size: 2rem;
    }

    .game-details {
        align-items: center;
    }

    .game-details h1 {
        font-size: 20px;
    }

    .life-container {
        height: 100%;
        width: 150px;
    }

    .life-container i {
        font-size: 25px;
    }

    .grid-container {
        width: 100%;
        /* height: 55vh; */
        margin: 0px !important;
    }
}

@media only screen and (max-width: 375px) {

    #topic-menu h1 {
        font-size: 40px;
        margin-bottom: 0px;
    }

    /* .grid-container {
        height: 50vh;
    } */

}
</style>
  
  