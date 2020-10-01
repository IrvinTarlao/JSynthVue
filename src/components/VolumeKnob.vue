<template>
    <div class="hello">
        <div class="box">
            <round-slider
                v-model="sliderValue"
                start-angle="0"
                end-angle="+270"
                line-cap="butt"
                radius="30"
                width="4"
                step="10"
                animation="true"
                handle-shape="square"
                handle-size="4"
                show-tooltip="false"
                path-color="#dadae4"
                range-color="#dadae4"
                mouse-scroll-action="true"
            />
            <div :style="{ position: 'absolute', color: 'grey', width: '100%', textAlign: 'center' }">{{ sliderValue / 10 }}</div>
        </div>
    </div>
</template>

<script>
import RoundSlider from "vue-round-slider";

export default {
    name: "VolumeKnob",
    components: {
        RoundSlider,
    },
    props: {
        volume: {
            type: Number,
        }
    },
    data() {
        return {
            sliderValue: this.volume,
        };
    },
    watch: {
        sliderValue: {
            immediate: true,
            handler(value) {
                this.$emit('volume', value);
            },
        },
    },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.box {
    padding: 0px;
    background-color: #dadae4;
    border-radius: 50%;
    display: flex;
    align-items: center;
    position: relative;
}
</style>
