<template>
    <div :class="getClass" @click="()=>this.osc.start()">
        <button>{{ pressed }}</button>
        <div class="">{{ uniqueKey.note }}</div>
        <div class="">{{ uniqueKey.frequency }}</div>
    </div>
</template>

<script>

const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
// const osc = audioCtx.createOscillator();
// const gainNode = audioCtx.createGain();
// gainNode.gain.value = 1;
// osc.type = "square";
// osc.frequency.value = 440;
// osc.start();
// console.log("la");

//     console.log("ici");
//     osc.connect(gainNode);
//     gainNode.connect(audioCtx.destination);


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
    },
    methods: {
        debug() {
            console.log(this.uniqueKey.keyboard);
        },
        keydown() {
            this.pressed = true;
            this.gainNode.gain.value = 0.1;
            this.osc.type = "sine";
            this.osc.frequency.value = this.uniqueKey.frequency;
            this.osc.connect(this.gainNode);
            this.gainNode.connect(audioCtx.destination);
        },
        keyup() {
            this.pressed = false;
            this.osc.disconnect();
        },
    },
    mounted() {
        window.addEventListener("keydown", (ev) => {
            if (ev.key === this.uniqueKey.keyboard) this.keydown();
        });
        window.addEventListener("keyup", (ev) => {
            if (ev.key === this.uniqueKey.keyboard && this.pressed) this.keyup();
        });
    },
    computed: {
        getClass() {
            return this.pressed ? "whiteKeyPressed" : "whiteKey";
        },
    },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.whiteKey {
    background-color: white;
    border: 2px solid grey;
    width: calc(100% / 8);
    height: 100%;
}

.whiteKeyPressed {
    background-color: rgb(172, 172, 172);
    border: 2px solid grey;
    width: calc(100% / 8);
    height: 100%;
}
.inputStyle {
    width: 1%;
    border: none;
}
</style>
