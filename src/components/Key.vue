<template>
    <div :class="getClass" @click="debug()">
        <button>{{ pressed }}</button>
        <div class="">{{ uniqueKey.note }}</div>
        <div class="">{{ uniqueKey.frequency }}</div>
    </div>
</template>

<script>
export default {
    name: "key",
    data() {
        return {
            pressed: false,
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
        },
        keyup() {
            this.pressed = false;
        },
    },
    mounted() {
        let self = this;
        window.addEventListener("keydown", function(ev) {
            if (ev.key === self.uniqueKey.keyboard) self.keydown();
        });
        window.addEventListener("keyup", function(ev) {
            if (ev.key === self.uniqueKey.keyboard && self.pressed) self.keyup();
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
