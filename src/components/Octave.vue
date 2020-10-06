<template>
<div :style="{color: 'white'}">
  <div>octave</div>
  <input type="number" v-model="octave" min="0" max="8">
</div>
</template>

<script>
import {BASE_OCTAVE} from './const';

export default {
    name: "octave",
    data() {
        return {
            octave: BASE_OCTAVE
        };
    },
    watch: {
        octave: {
            immediate: true,
            handler(value) {
                this.$emit("octave", parseInt(value));
            },
        },
    },

}
</script>

<style>

</style>