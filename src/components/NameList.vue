<template>
  <section class="container">
    <section class="user-names">
      <h2>Names</h2>
      <input type="text" v-model="enteredNameValue" @keyup.enter="addName" />
      <button @click="addName">Add Name</button>
      <button @click="selectAll">Select All</button>
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
      <div class="listWrapper">
        <ul class="list" ref="list">
          <li v-for="(name, index) in namesToChooseFrom" :key="index">
            {{ name }}
          </li>
        </ul>
      </div>
      <button @click="triggerAnimation">GO</button>
    </div>
  </section>
</template>

<script lang="ts">
import { ref, watch, onMounted, type Ref } from "vue";
import { gsap } from "gsap";

export default {
  setup() {
    const enteredNameValue = ref<String>("");
    const names = ref<String[]>([]);
    const namesToChooseFrom = ref<String[]>([]);
    const chosenName = ref<String>("");
    const list: Ref = ref(null);

    function selectAll() {
      namesToChooseFrom.value = names.value;
    }

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

    const triggerAnimation = () => {
      const liHeight = list.value.clientHeight;
      let counter = 1;
      const chosenNumber = Math.floor(
        Math.random() * namesToChooseFrom.value.length
      );

      if (counter == namesToChooseFrom.value.length) {
        counter = 1;
        gsap.set(list.value, { y: 0 });
      }

      gsap.to(list.value, {
        y: chosenNumber * (0 - liHeight * counter),
        ease: "elastic.out(8, 0)",
      });
      counter++;
    };

    watch(
      names,
      (newVal) => {
        localStorage.setItem("names", JSON.stringify(newVal));
      },
      { deep: true }
    );

    onMounted(() => {
      names.value = JSON.parse(localStorage.getItem("names") || "");
    });

    return {
      enteredNameValue,
      names,
      namesToChooseFrom,
      chosenName,
      addName,
      removeName,
      triggerAnimation,
      selectAll,
      list,
    };
  },
};
</script>

<style scoped>
.listWrapper {
  font-family: Oswald;
  font-size: 36px;
  text-transform: uppercase;
  color: white;
  height: 1em;
  line-height: 1em;
  overflow: hidden;
  margin: 0;
  -webkit-font-smoothing: antialiased;
  -webkit-backface-visibility: hidden;
  backface-visibility: hidden;
}
.list {
  padding: 0;
  display: inline-block;
  vertical-align: middle;
  height: 1em;
  line-height: 1em;
}
.list li {
  margin: 0;
  padding: 0;
  list-style: none;
  color: white;
  height: 1em;
  line-height: 1em;
  margin: 0;
  display: block;
}
.namestochoosefrom {
  padding: 20px;
  margin: 20px;
  background-color: #9c9892;
  color: black;
  font-size: larger;
  font-weight: bold;
}

#spinner {
  margin-top: 150px;
}

.user-names {
  width: 50%;
}

.container {
  display: flex;
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
