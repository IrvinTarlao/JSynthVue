<template>
<div :style="{color: 'white'}">
  <div>octave</div>
  <input type="number" v-model="octave" min="0" max="8">
</div>
</template>

<script>
export default {
    name: "octave",
    data() {
        return {
            octave: 4
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