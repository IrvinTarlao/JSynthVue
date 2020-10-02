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
        <div :style="{border:'2px solid red'}">
            <div :style="{border:'2px solid blue'}">
                <VolumeKnob @volume="getVolume" v-bind:volume="volume" />
            </div>
            <div>
                <WaveType @waveType="getWaveType" v-bind:waveType="waveType" />
            </div>
            <div>
                <Octave @octave="getOctave" v-bind:octave="octave" />
            </div>
        </div>
        <Keyboard v-bind:volume="volume" v-bind:waveType="waveType" v-bind:startOSC="start" v-bind:octave="octave"/>
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
        Octave
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
            octave: null
        };
    },
    // watch: {
    //     octave: {
    //         immediate: true,
    //         handler(value) {
    //             console.log(value)
    //         },
    //     },
    // },
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
}
</style>
