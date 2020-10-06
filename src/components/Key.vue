<template>
    <div class="keyContainer">
        <div class="outerBlackKey" :class="getOuterBlackKeyPressed" :style="{ justifyContent: uniqueKey.align }" v-if="uniqueKey.color === 'black'" >
            <div class="innerBlackKey" :class="getInnerBlackKeyPressed">
                <div class="blackKeyNote">{{ uniqueKey.note }}</div>
            </div>
        </div>
        <div class="outerWhiteKey" :class="getOuterWhiteKeyPressed" v-if="uniqueKey.color === 'white'">
            <div class="innerWhiteKey" :class="getInnerWhiteKeyPressed">{{ uniqueKey.note }}</div>
        </div>
    </div>
</template>

<script>
import { BASE_OCTAVE } from "./const";
const audioCtx = new (window.AudioContext || window.webkitAudioContext)();

export default {
    name: "key",
    data() {
        return {
            osc: audioCtx.createOscillator(),
            gainNode: audioCtx.createGain(),
        };
    },
    props: {
        uniqueKey: {
            type: Object,
        },
        startOSC: {
            type: Boolean,
        },
        volume: {
            type: Number,
        },
        octave: {
            type: Number,
        },
        waveType: {
            type: String,
        },
        pressed: {
            type: Boolean,
        },
    },
    computed: {
        getOuterWhiteKeyPressed() {
            return this.pressed && this.startOSC ? "outerWhiteKeyPressed" : "";
        },
        getInnerWhiteKeyPressed() {
            return this.pressed && this.startOSC ? "innerWhiteKeyPressed" : "";
        },
        getOuterBlackKeyPressed() {
            return this.pressed && this.startOSC ? "outerBlackKeyPressed" : "";
        },
        getInnerBlackKeyPressed() {
            return this.pressed && this.startOSC ? "innerBlackKeyPressed" : "";
        },
        getOctave() {
            const octDiff = this.octave - BASE_OCTAVE;
            const factor = Math.pow(2, Math.abs(octDiff));
            let frequencyToPlay;
            if (this.octave === BASE_OCTAVE) frequencyToPlay = this.uniqueKey.frequency;
            else if (this.octave > BASE_OCTAVE) frequencyToPlay = this.uniqueKey.frequency * factor;
            else if (this.octave < BASE_OCTAVE) frequencyToPlay = this.uniqueKey.frequency / factor;
            return frequencyToPlay;
        },
    },
    watch: {
        startOSC: {
            immediate: true,
            handler(value) {
                if (value) this.osc.start();
            },
        },
        pressed: {
            immediate: true,
            handler(value) {
                if (value) {
                    this.osc.type = this.waveType;
                    this.osc.frequency.value = this.getOctave;
                    this.gainNode.gain.value = this.volume / 100;
                    this.osc.connect(this.gainNode);
                    this.gainNode.connect(audioCtx.destination);
                } else {
                    this.osc.disconnect();
                }
            },
        },
    },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.keyContainer {
    height: 100%;
    margin: 1px;
}

.outerWhiteKey {
    height: 100%;
    width: 100%;
    border-radius: 5px;
    background: #dadae4;
    box-shadow: inset 5px 5px 10px #fbfbff, inset -5px -5px 10px #fbfbff, 0px 0px 5px 0px rgba(23, 20, 25, 1);
    -webkit-box-shadow: inset 5px 5px 10px #fbfbff, inset -5px -5px 10px #fbfbff, 0px 0px 5px 0px rgba(23, 20, 25, 1);
    -moz-box-shadow: inset 5px 5px 10px #fbfbff, inset -5px -5px 10px #fbfbff, 0px 0px 5px 0px rgba(23, 20, 25, 1);
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 0 2px;
}

.outerWhiteKeyPressed {
    background: #cfcfd6b9;
    box-shadow: inset 0px -5px 10px #75757a, 0px 0px 5px 0px rgba(23, 20, 25, 1);
    -webkit-box-shadow: inset 0px -5px 10px #75757a, 0px 0px 5px 0px rgba(23, 20, 25, 1);
    -moz-box-shadow: inset 0px -5px 10px #75757a, 0px 0px 5px 0px rgba(23, 20, 25, 1);
}

.innerWhiteKey {
    height: 80%;
    width: 60%;
    border-radius: 36px;
    background: #dadae4;
    box-shadow: 5px 5px 10px #85858b, -5px -3px 10px #ffffff, inset 2px 2px 5px #ffffff;
    text-align: center;
    padding: 20px;
    font-size: 0.8em;
}

.innerWhiteKeyPressed {
    background: #cfcfd6b9;
    box-shadow: 5px 5px 10px #75757a, -5px -3px 10px #f3f3f3, inset 2px 2px 5px #f3f3f3;
}

.outerBlackKey {
    height: 100%;
    width: 100%;
    border-radius: 5px;
    background: #dadae4;
    box-shadow: inset 5px 5px 10px #fbfbff, inset -5px -5px 10px #fbfbff, 0px 0px 5px 0px rgba(23, 20, 25, 1);
    -webkit-box-shadow: inset 5px 5px 10px #fbfbff, inset -5px -5px 10px #fbfbff, 0px 0px 5px 0px rgba(23, 20, 25, 1);
    -moz-box-shadow: inset 5px 5px 10px #fbfbff, inset -5px -5px 10px #fbfbff, 0px 0px 5px 0px rgba(23, 20, 25, 1);
    display: flex;
    align-items: center;
    margin: 0 2px;
    padding: 20px;
}

.outerBlackKeyPressed {
    background: #cfcfd6b9;
    box-shadow: inset 0px -5px 10px #75757a, 0px 0px 5px 0px rgba(23, 20, 25, 1);
    -webkit-box-shadow: inset 0px -5px 10px #75757a, 0px 0px 5px 0px rgba(23, 20, 25, 1);
    -moz-box-shadow: inset 0px -5px 10px #75757a, 0px 0px 5px 0px rgba(23, 20, 25, 1);
}

.innerBlackKey {
    border-radius: 50%;
    height: 60px;
    width: 60px;
    background: #dadae4;
    box-shadow: 5px 5px 8px #85858b, -5px -5px 8px #ffffff;
    padding: 3px;
    color: white;
}

.innerBlackKeyPressed {
    background: #cfcfd6b9;
    box-shadow: 5px 5px 10px #75757a, -5px -3px 10px #f3f3f3, inset 2px 2px 5px #f3f3f3;
}

.blackKeyNote {
    width: 100%;
    height: 100%;
    border-radius: 50%;
    background-color: #171419;
    text-align: center;
    padding: 17px;
    font-size: 0.8em;
}
</style>
