<template>
  <canvas id="canvas" width="1250" height="375"></canvas>
</template>

<script>
export default {
  data: function () {
    return {
      canvas: document.getElementById("canvas"),
    };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
canvas {
  border: 1px solid black;
}
</style>
