<template>
  <div class="wrapper">
    <div class="container">
      <div class="nameslist">
        <NameList />
      </div>
      <div class="animation">
        <AnimationSpin />
      </div>
      <div class="log">
        <NameLog />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import NameList from "./components/NameList.vue";
import AnimationSpin from "./components/AnimationSpin.vue";
import NameLog from "./components/NameLog.vue";

export default {
  components: {
    NameList,
    AnimationSpin,
    NameLog,
  },
  setup() {},
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Roboto&display=swap");

body {
  background-color: black;
  font-family: "roboto", "sans-sarif";
}

.wrapper {
  display: flex;
  justify-content: center;
  margin-top: 100px;
}

.container {
  display: flex;
}

.animation {
  margin-left: 5em;
  margin-right: 5em;
}
</style>
