<template>
  <div class="log">
    <h2>Log</h2>
    <button @click="toggleLog">Open Log</button>
    <ul class="log-list" v-if="!isActiveLog">
      <li v-for="(name, index) in chosenNames" :key="index">
        {{ name }}
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import { ref } from "vue";
import NameList from "./NameList.vue";

export default {
  props: ["chosenNames"],
  setup() {
    const isActiveLog = ref<Boolean>(true);

    const toggleLog = () => {
      isActiveLog.value = isActiveLog.value ? false : true;
    };

    return {
      isActiveLog,
      toggleLog,
    };
  },
};
</script>

<style scoped>
.log-list {
  margin-top: 50px;
  height: 200px;
  font-family: "Roboto", sans-serif;
  font-size: 20px;
  overflow: auto;
}
.log {
  color: white;
  width: 100%;
}
</style>
