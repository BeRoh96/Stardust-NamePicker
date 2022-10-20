<template>
  <section class="container">
    <section class="user-names">
      <h2>Names</h2>
      <input type="text" v-model="enteredNameValue" @keyup.enter="addName" />
      <button @click="addName">Add Name</button>
      <button @click="toggleSelectAll">Select All</button>
      <ul>
        <li v-for="(name, index) in names" :key="index">
          <input type="checkbox" :value="name" v-model="namesToChooseFrom" />
          <label for="name">{{ name }}</label>
          <button id="destroy" @click="removeName(name)">X</button>
        </li>
      </ul>
    </section>
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
    <div class="log">
      <h2>Log</h2>
      <button @click="toggleLog">Open Log</button>
      <ul class="log-list" v-if="!isActiveLog">
        <li v-for="(name, index) in chosenNames" :key="index">
          {{ name }}
        </li>
      </ul>
    </div>
  </section>
</template>

<script lang="ts">
import {
  ref,
  watch,
  onMounted,
  type Ref,
  type ComputedRef,
  computed,
} from "vue";
import { gsap } from "gsap";

export default {
  setup() {
    const enteredNameValue = ref<String>("");
    const names = ref<String[]>([]);
    const namesToChooseFrom = ref<String[]>([]);
    const chosenNames = ref<String[]>([]);
    const list: Ref<HTMLElement | null> = ref(null);
    const isActiveSelectAll = ref<Boolean>(false);
    const isActiveLog = ref<Boolean>(true);

    function addName() {
      if (enteredNameValue.value.trim() === "") {
        return;
      }
      names.value.push(enteredNameValue.value);
      enteredNameValue.value = "";
    }

    function removeName(name: String) {
      names.value.splice(names.value.indexOf(name), 1);
    }

    const namesToRender: ComputedRef = computed(() => {
      const arrayToShuffle = [
        ...namesToChooseFrom.value,
        ...namesToChooseFrom.value,
        ...namesToChooseFrom.value,
        ...namesToChooseFrom.value,
        ...namesToChooseFrom.value,
      ];
      return arrayToShuffle.sort(() => Math.random() - 0.5);
    });

    const toggleSelectAll = () => {
      isActiveSelectAll.value = isActiveSelectAll.value ? false : true;

      if (isActiveSelectAll.value) {
        namesToChooseFrom.value = names.value;
      } else {
        namesToChooseFrom.value = [];
      }
    };

    const toggleLog = () => {
      isActiveLog.value = isActiveLog.value ? false : true;
    };

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
      chosenNames.value.unshift(namesToRender.value[chosenNumber]);
    };

    watch(
      names,
      (newVal) => {
        localStorage.setItem("names", JSON.stringify(newVal));
        localStorage.setItem("chosenNames", JSON.stringify(newVal));
      },
      { deep: true }
    );

    onMounted(() => {
      names.value = JSON.parse(localStorage.getItem("names") || "");
      chosenNames.value = JSON.parse(localStorage.getItem("chosenNames") || "");
    });

    return {
      enteredNameValue,
      names,
      namesToChooseFrom,
      namesToRender,
      chosenNames,
      addName,
      removeName,
      triggerAnimation,
      list,
      isActiveLog,
      toggleSelectAll,
      toggleLog,
    };
  },
};
</script>

<style scoped lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Roboto&display=swap");

.log-list {
  margin-top: 50px;
  height: 200px;
  font-family: "Roboto", sans-serif;
  font-size: 20px;
  overflow: auto;
}
.log {
  position: absolute;
  margin-left: 900px;
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

#spinner {
  margin-top: 150px;
}

.user-names {
  width: 50%;
}

.container {
  display: flex;
  // width: 100%;
  width: 1300px;
}

button:hover,
button:active {
  background-color: #a37125;
  border-color: #a37125;
  box-shadow: 1px 1px 4px rgba(0, 0, 0, 0.26);
}

button {
  font: inherit;
  cursor: pointer;
  margin-left: 60px;
  border: 1px solid #ff9900;
  background-color: #ff9900;
  color: white;
  padding: 0.05rem 1rem;
  box-shadow: 1px 1px 2px rgba(0, 0, 0, 0.26);
}

.container {
  color: white;
}

.container button {
  margin-top: 20px;
}

.container li {
  list-style: none;
}

.container li label {
  margin-left: 20px;
  font-size: 20px;
}
</style>
