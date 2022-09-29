<template>
  <section class="container">
    <section class="user-names">
      <h2>Names</h2>
      <input type="text" v-model="enteredNameValue" @keyup.enter="addName" />
      <button @click="addName">Add Name</button>
      <ul>
        <li v-for="(name, key, index) in names" :key="index">
          <input
            :id="name"
            type="checkbox"
            :value="name"
            v-model="namesToChooseFrom"
          />
          <label for="name">{{ name }}</label>
          <button id="destroy" @click="removeName(name)">X</button>
        </li>
      </ul>
      <span>{{ namesToChooseFrom }}</span>
      <div id="picked-name">
        <button class="divContent" id="pickNameButton" @click="pickName">
          GO!
        </button>
        <span class="divContent" id="chosenName">{{ chosenName }}</span>
      </div>
    </section>
  </section>
</template>

<script>
import { ref, watch, onMounted } from "vue";

export default {
  setup() {
    const enteredNameValue = ref("");
    const names = ref([]);
    const namesToChooseFrom = ref([]);
    const chosenName = ref("");

    function addName() {
      if (enteredNameValue.value.trim() === "") {
        return;
      }
      names.value.push(enteredNameValue.value);
      enteredNameValue.value = "";
    }

    function removeName(name) {
      names.value.splice(names.value.indexOf(name), 1);
    }

    function pickName() {
      const chosenNumber = Math.floor(
        Math.random() * namesToChooseFrom.value.length
      );
      chosenName.value = namesToChooseFrom[chosenNumber];
    }

    watch(
      names,
      (newVal) => {
        localStorage.setItem("names", JSON.stringify(newVal));
      },
      { deep: true }
    );

    onMounted(() => {
      names.value = JSON.parse(localStorage.getItem("names")) || [];
    });

    return {
      enteredNameValue,
      names,
      namesToChooseFrom,
      chosenName,
      addName,
      removeName,
      pickName,
    };
  },
  //     pickName() {
  //       var chosenNumber = Math.floor(
  //         Math.random() * this.namesToChooseFrom.length,
  //       );
  //       this.chosenName = this.namesToChooseFrom[chosenNumber];
  //     },
  //   },
  // });
};
</script>

<style scoped>
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
