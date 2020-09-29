<template>
    <div id="app">
        <Frame />
    </div>
</template>

<script>
import Frame from "./components/Frame.vue";

export default {
    name: "App",
    data() {
        return {

        };
    },
    components: {
        Frame,
    },
};
</script>

<style>

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
}
</style>
