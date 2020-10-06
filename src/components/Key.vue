<template>
    <div class="keyContainer">
        <div :class="getOuterBlackClass" v-if="uniqueKey.color === 'black'" v-bind:style="{ justifyContent: uniqueKey.align, width: '100%' }">
            <div :class="getInnerBlackClass">
                <div v-bind:style="{ width: '100%', height: '100%', borderRadius: '50%', backgroundColor: '#171419', textAlign: 'center', padding: '15px' }">{{ uniqueKey.note }}</div>
            </div>
        </div>
        <div :class="getOuterWhiteClass" v-if="uniqueKey.color === 'white'">
            <div :class="getInnerWhiteClass" v-bind:style="{ textAlign: 'center', padding: '20px' }">{{ uniqueKey.note }}</div>
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
        getOuterWhiteClass() {
            return this.pressed && this.startOSC ? "outerWhiteKeyPressed" : "outerWhiteKey";
        },
        getInnerWhiteClass() {
            return this.pressed && this.startOSC ? "innerWhiteKeyPressed" : "innerWhiteKey";
        },
        getOuterBlackClass() {
            return this.pressed && this.startOSC ? "outerBlackKeyPressed" : "outerBlackKey";
        },
        getInnerBlackClass() {
            return this.pressed && this.startOSC ? "innerBlackKeyPressed" : "innerBlackKey";
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
    height: 100%;
    width: 100%;
    border-radius: 5px;
    background: #cfcfd6b9;
    box-shadow: inset 0px -5px 10px #75757a, 0px 0px 5px 0px rgba(23, 20, 25, 1);
    -webkit-box-shadow: inset 0px -5px 10px #75757a, 0px 0px 5px 0px rgba(23, 20, 25, 1);
    -moz-box-shadow: inset 0px -5px 10px #75757a, 0px 0px 5px 0px rgba(23, 20, 25, 1);
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 0 2px;
}

.innerWhiteKey {
    height: 80%;
    width: 60%;
    border-radius: 36px;
    background: #dadae4;
    box-shadow: 5px 5px 10px #85858b, -5px -3px 10px #ffffff, inset 2px 2px 5px #ffffff;
}

.innerWhiteKeyPressed {
    height: 80%;
    width: 60%;
    border-radius: 36px;
    background: #cfcfd6b9;
    box-shadow: 5px 5px 10px #75757a, -5px -3px 10px #f3f3f3, inset 2px 2px 5px #f3f3f3;
}

.outerBlackKey {
    height: 100%;
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
    height: 100%;
    border-radius: 5px;
    background: #cfcfd6b9;
    box-shadow: inset 0px -5px 10px #75757a, 0px 0px 5px 0px rgba(23, 20, 25, 1);
    -webkit-box-shadow: inset 0px -5px 10px #75757a, 0px 0px 5px 0px rgba(23, 20, 25, 1);
    -moz-box-shadow: inset 0px -5px 10px #75757a, 0px 0px 5px 0px rgba(23, 20, 25, 1);
    display: flex;
    align-items: center;
    margin: 0 2px;
    padding: 20px;
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
    border-radius: 50%;
    height: 60px;
    width: 60px;
    background: #cfcfd6b9;
    box-shadow: 5px 5px 10px #75757a, -5px -3px 10px #f3f3f3, inset 2px 2px 5px #f3f3f3;
    padding: 3px;
    color: white;
}
</style>
