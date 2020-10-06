<template>
    <div class="frame">
        <div v-if="!start" @click="startOscs" class="clickStart">
            click to start
        </div>
        <div class="filters">
            <div>
                <div>volume</div>
                <VolumeKnob @volume="getVolume" v-bind:volume="volume" />
            </div>
            <div>
                <div>wave type</div>
                <WaveType @waveType="getWaveType" v-bind:waveType="waveType" />
            </div>
            <div>
                <Octave @octave="getOctave" v-bind:octave="octave" />
            </div>
        </div>
        <Keyboard v-bind:volume="volume" v-bind:waveType="waveType" v-bind:startOSC="start" v-bind:octave="octave" />
    </div>
</template>

<script>
import Keyboard from "./Keyboard";
import VolumeKnob from "./VolumeKnob";
import WaveType from "./WaveType";
import Octave from "./Octave";

export default {
    name: "frame",
    components: {
        Keyboard,
        VolumeKnob,
        WaveType,
        Octave,
    },
    methods: {
        startOscs() {
            return (this.start = true);
        },
        getVolume(value) {
            this.volume = value;
        },
        getWaveType(value) {
            this.waveType = value;
        },
        getOctave(value) {
            this.octave = value;
        },
    },
    data() {
        return {
            start: false,
            volume: 10,
            waveType: "sine",
            octave: null,
        };
    },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.frame {
    background-color: rgb(61, 61, 61);
    width: 100vw;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    color: white;
}

.clickStart {
    position: absolute;
    width: 100%;
    height: 50%;
    font-size: 3em;
    background-color: rgba(0, 0, 0, 0.8);
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    color: white;
    z-index: 9;
}

.filters {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}
</style>
