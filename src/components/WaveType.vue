<template>
    <div class="slidecontainer">
        <div :style="{display: 'flex', justifyContent: 'space-between', color: 'white', fontSize: '0.5em'}">
            <div>sine</div>
            <div>square</div>
            <div>saw</div>
            <div>triangle</div>
        </div>
        <input type="range" min="1" max="100" v-model="position" class="slider" id="myRange" step="33" />
    </div>
</template>

<script>
export default {
    name: "WaveType",
    props: {
        waveType: {
            type: String,
        },
    },
    data() {
        return {
            position: 0, //sine
        };
    },
    watch: {
        position: {
            immediate: true,
            handler(value) {
                this.$emit("waveType", parseInt(value));
            },
        },
    },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.slidecontainer {
    width: 100%; /* Width of the outside container */
}

/* The slider itself */
.slider {
    -webkit-appearance: none; /* Override default CSS styles */
    appearance: none;
    width: 100%; /* Full-width */
    height: 25px; /* Specified height */
    background: white; /* Grey background */
    outline: none; /* Remove outline */
    opacity: 1; /* Set transparency (for mouse-over effects on hover) */
    -webkit-transition: 0.2s; /* 0.2 seconds transition on hover */
    transition: opacity 0.2s;
}

/* Mouse-over effects */
.slider:hover {
    opacity: 0.8; /* Fully shown on mouse-over */
}

/* The slider handle (use -webkit- (Chrome, Opera, Safari, Edge) and -moz- (Firefox) to override default look) */
.slider::-webkit-slider-thumb {
    -webkit-appearance: none; /* Override default look */
    appearance: none;
    width: 5px; /* Set a specific slider handle width */
    height: 25px; /* Slider handle height */
    background: grey; /* Green background */
    cursor: pointer; /* Cursor on hover */
}

.slider::-moz-range-thumb {
    width: 25px; /* Set a specific slider handle width */
    height: 25px; /* Slider handle height */
    background: grey; /* Green background */
    cursor: pointer; /* Cursor on hover */
}
</style>
