<template>
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
                <Key v-bind:uniqueKey="blackKey" v-bind:startOSC="startOSC" v-bind:volume="volume" v-bind:waveType="waveType" v-bind:octave="octave" v-bind:pressed="sendPressed(blackKey.keyboard)"/>
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
                <Key v-bind:uniqueKey="whiteKey" v-bind:startOSC="startOSC" v-bind:volume="volume" v-bind:waveType="waveType" v-bind:octave="octave" v-bind:pressed="sendPressed(whiteKey.keyboard)"/>
            </div>
        </div>
    </div>
</template>

<script>
import Key from "./Key";

export default {
    name: "keyboard",
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
            pressedKeys: []
        }
    },
    props: {
        volume: {
            type: Number,
        },
        octave: {
            type: Number,
        },
        waveType: {
            type: String,
        },
        startOSC: {
            type: Boolean,
        },
    },
    components: {
        Key
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
    methods: {
        keyDown(key) {
            this.pressedKeys.push(key);
        },
        keyUp(key) {
            this.pressedKeys = this.pressedKeys.filter(pressedKey => pressedKey !== key)
        },
        sendPressed(key) {
            if (this.pressedKeys.includes(key)) return true
            else return false;
        }
    },
    mounted() {
        window.addEventListener("keydown", (ev) => {
            if (ev.repeat) return;
            this.keyDown(ev.key);
        });
        window.addEventListener("keyup", (ev) => {
            this.keyUp(ev.key);
        });
    },
    destroyed() {
        window.removeEventListener("keydown", this.keyDown);
        window.removeEventListener("keyup", this.keyUp);
    },
};
</script>

<style>
.keyboard {
    width: 90%;
    height: 50%;
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
