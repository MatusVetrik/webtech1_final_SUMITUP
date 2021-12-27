<template>
  <body>
    <the-header></the-header>
    <my-canvas></my-canvas>
  </body>
</template>

<script>
import MyCanvas from "./components/MyCanvas.vue";
import TheHeader from "./components/TheHeader.vue";

export default {
  components: {
    MyCanvas,
    TheHeader,
  },
  data() {
    return {};
  },
  methods: {},
};
</script>

<style>
body {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background: rgb(0, 0, 0);
}
</style>
