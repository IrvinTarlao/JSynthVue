<template>
    <div class="frame">
        <div
            v-if="!start"
            @click="() => startOscs()"
            v-bind:style="{
                position: 'absolute',
                width: '90%',
                height: '50%',
                fontSize: '3em',
                backgroundColor: 'rgba(0, 0, 0, 0.8)',
                display: 'flex',
                justifyContent: 'center',
                alignItems: 'center',
                cursor: 'pointer',
                color: 'white',
            }"
        >
            click to start
        </div>
        <div class="keyboard">            
            <div class="upperKeyboard">
                <div
                    v-bind:style="{
                        width: '100%',
                        height: '100%',
                    }"
                    v-for="(blackKey, i) in blackKeys"
                    v-bind:key="i"
                >
                    <Key v-bind:uniqueKey="blackKey" v-bind:startOSC="start" />
                </div>
            </div>
            <div class="lowerKeyboard">
                <div
                    v-bind:style="{
                        width: '100%',
                        height: '100%',
                    }"
                    v-for="(whiteKey, i) in whiteKeys"
                    v-bind:key="i"
                >
                    <Key v-bind:uniqueKey="whiteKey" v-bind:startOSC="start" />
                </div>
            </div>
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
    methods: {
        startOscs() {
            return (this.start = true);
        },
    },
    data() {
        return {
            start: false,
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
    computed: {
        whiteKeys: function() {
            return this.keysList.filter(function(k) {
                return k.color === "white";
            });
        },
        blackKeys: function() {
            return this.keysList.filter(function(k) {
                return k.color === "black";
            });
        },
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
    /* border: 2px solid black; */
    padding: 5px;
    overflow: auto;
}

.upperKeyboard {
    width: 100%;
    display: flex;
    justify-content: space-between;
    margin-bottom: 5px;
}

.lowerKeyboard {
    width: 100%;
    height: 75%;
    display: flex;
}
</style>
