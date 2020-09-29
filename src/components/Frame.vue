<template>
    <div class="frame">
        <div class="keyboard">
            <Key v-for="keyNote in keysList" v-bind:keyNote="keyNote" />
        </div>
    </div>
</template>

<script>
import Key from "./Key";

export default {
    name: "frame",
    props: {
        msg: String,
    },
    components: {
        Key,
    },
    data() {
        return {
            keysList: [
                { note: "F", keyboard: "f", color: "white", frequency: 349.23 },
                { note: "G", keyboard: "g", color: "white", frequency: 392 },
                { note: "A", keyboard: "h", color: "white", frequency: 440 },
                { note: "B", keyboard: "j", color: "white", frequency: 493.88 },
                { note: "C", keyboard: "k", color: "white", frequency: 523.25 },
                { note: "D", keyboard: "l", color: "white", frequency: 587.33 },
                { note: "E", keyboard: "m", color: "white", frequency: 659.25 },
                { note: "F#", align: "flex-end", keyboard: "t", color: "black", frequency: 369.99 },
                { note: "G#", align: "center", keyboard: "y", color: "black", frequency: 415.3 },
                { note: "A#", align: "flex-start", keyboard: "u", color: "black", frequency: 466.16 },
                { note: "C#", align: "flex-end", keyboard: "o", color: "black", frequency: 554.37 },
                { note: "D#", align: "flex-start", keyboard: "p", color: "black", frequency: 622.25 },
            ],
        };
    },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.frame {
    background-color: rgb(61, 61, 61);
    width: 100vw;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
}

.keyboard {
    width: 90%;
    height: 50%;
    border: 2px solid black;
    background-color: red;
    display: flex;
}
</style>
