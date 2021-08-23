<template>
  <div id="app">
    <div class="container">
      <Simon :difficulty="difficulty" />
      <div class="game-info">
        <Difficulty :defDifficulty="defDifficulty" :difficulty="difficulty" />
      </div>
    </div>
  </div>
</template>

<script>
import Simon from "./components/Simon";
import Difficulty from "./components/Difficulty";

export default {
  name: "App",
  data() {
    return {
      difficulty: "easy",
    };
  },
  methods: {
    defDifficulty(mode) {
      this.difficulty = mode;
    },
  },
  components: {
    Simon,
    Difficulty,
  },
};
</script>


<style lang="scss">
@import "./assets/styles/reset";
@import "./assets/styles/vars";
@import "./assets/styles/mixins";

@import "./assets/styles/app";
@import "./assets/styles/simon";
@import "./assets/styles/difficulty";
</style>
