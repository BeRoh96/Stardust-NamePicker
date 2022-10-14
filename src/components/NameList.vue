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
      <p>
        <span class="list">
          <span>{{ chosenName }}</span>
        </span>
      </p>
      <button @click="pickName">GO</button>
    </div>
  </section>
</template>

<script lang="ts">
import { ref, watch, onMounted } from "vue";
import gsap from "gsap";

export default {
  setup() {
    const enteredNameValue = ref<String>("");
    const names = ref<String[]>([]);
    const namesToChooseFrom = ref<String[]>([]);
    const chosenName = ref<String>("");

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

    function pickName() {
      const chosenNumber = Math.floor(
        Math.random() * namesToChooseFrom.value.length
      );
      chosenName.value = namesToChooseFrom.value[chosenNumber];
    }

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
      pickName,
      selectAll,
    };
  },
};
</script>

<style scoped>
p {
  font-family: Oswald;
  font-size: 36px;
  text-transform: uppercase;
  color: white;
  height: 1em;
  line-height: 1em;
  overflow: hidden;
  margin-top: 40px;
  -webkit-font-smoothing: antialiased;
  font-smoothing: antialiased;
  -webkit-backface-visibility: hidden;
  backface-visibility: hidden;
}
span.list {
  margin: -0.25em 0 0 0;
  padding: 0;
  display: inline-block;
  vertical-align: middle;
  height: 1em;
  line-height: 1em;
}
span.list span {
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
