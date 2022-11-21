<template>
  <div class="randomizer">
    <h2>Randomizer</h2>
    <div class="list-wrapper">
      <ul class="list" ref="list">
        <li v-for="(name, index) in namesToRender" :key="index">
          {{ name }}
        </li>
      </ul>
    </div>
    <button @click="triggerAnimation">GO</button>
  </div>
</template>

<script lang="ts">
import { type ComputedRef, computed, ref, type Ref } from "vue";
import { gsap } from "gsap";

export default {
  props: ["namesToChooseFrom", "chosenNames"],
  setup(props) {
    // const props = defineProps(["namesToChooseFrom"]);
    const list: Ref<HTMLElement | null> = ref(null);

    const namesToRender: ComputedRef = computed(() => {
      const arrayToShuffle = [
        ...props.namesToChooseFrom.value,
        ...props.namesToChooseFrom.value,
        ...props.namesToChooseFrom.value,
        ...props.namesToChooseFrom.value,
        ...props.namesToChooseFrom.value,
      ];
      return arrayToShuffle.sort(() => Math.random() - 0.5);
    });

    const triggerAnimation = () => {
      gsap.set(list.value, { opacity: 1 });
      const listHeight = list.value?.clientHeight;
      const memberCount = namesToRender.value.length;
      const chosenNumber = Math.floor(Math.random() * memberCount);

      if (listHeight) {
        const position = -1 * (chosenNumber * listHeight);
        const duration = chosenNumber * listHeight * 0.01;

        gsap.fromTo(
          list.value,
          { y: 0 },
          {
            y: position,
            ease: "elastic.out(1, 0.3)",
            duration: duration,
          }
        );
      }
      props.chosenNames.value.unshift(namesToRender.value[chosenNumber]);
    };

    return {
      triggerAnimation,
      namesToRender,
      list,
    };
  },
};
</script>

<style scoped lang="scss">
.randomizer {
  color: white;
  width: 100%;
  height: 100%;
}
.list-wrapper {
  font-family: "Roboto", sans-serif;
  font-size: 36px;
  text-transform: uppercase;
  color: black;
  height: 1em;
  line-height: 1em;
  overflow: hidden;
  margin: 0;
  -webkit-font-smoothing: antialiased;
  -webkit-backface-visibility: hidden;
  backface-visibility: hidden;
}

ul.list {
  margin: -0.25em 0 0 0;
  padding: 0;
  display: inline-block;
  vertical-align: middle;
  height: 1em;
  line-height: 1em;
  opacity: 0;
  & li {
    margin: 0;
    padding: 0;
    list-style: none;
    color: white;
    height: 1em;
    line-height: 1em;
    margin: 0;
    display: block;
  }
}
</style>
