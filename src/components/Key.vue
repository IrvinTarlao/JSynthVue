<template>
    <div :style="{ height: '100%', margin: '1px'}">
        <!-- width: uniqueKey.note === 'G#' ? '50%' : '100%'  -->
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
const audioCtx = new (window.AudioContext || window.webkitAudioContext)();

export default {
    name: "key",
    data() {
        return {
            pressed: false,
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
        waveType: {
            type: String,
        }
    },
    methods: {
        debug() {
            console.log(this.uniqueKey.keyboard);
        },
        keyDown() {
            this.pressed = true;
            this.osc.type = this.waveType;
            this.osc.frequency.value = this.uniqueKey.frequency;
            this.gainNode.gain.value = this.volume/100;
            this.osc.connect(this.gainNode);
            this.gainNode.connect(audioCtx.destination);
        },
        keyUp() {
            this.pressed = false;
            this.osc.disconnect();
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
    },
    watch: {
        startOSC: {
            immediate: true,
            handler(value) {
                if (value === true) this.osc.start();
            },
        },
    },
    mounted() {
        window.addEventListener("keydown", (ev) => {
            if (ev.repeat) return;
            if (ev.key === this.uniqueKey.keyboard) this.keyDown();
        });
        window.addEventListener("keyup", (ev) => {
            if (ev.key === this.uniqueKey.keyboard && this.pressed) this.keyUp();
        });
    },
    destroyed() {
        window.removeEventListener("keydown", this.keyDown());
        window.removeEventListener("keyup", this.keyUp());
    },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
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
