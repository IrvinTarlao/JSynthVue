<template>
    <div class="whiteKey">
        <div class="" @keydown="objTest">{{this.props.keyNote}}</div>
    </div>
</template>

<script>
export default {
    name: "key",
    data() {
        return {};
    },
    props: {
        keyNote: Object,
    },
    methods: {
        debug(event) {
            console.log(this.props.keyNote);
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
</style>
