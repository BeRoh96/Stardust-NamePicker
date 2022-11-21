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
  </section>
</template>

<script lang="ts">
import { ref, watch, onMounted, type Ref, provide } from "vue";

export default {
  setup() {
    const enteredNameValue = ref<String>("");
    const names = ref<String[]>([]);
    const namesToChooseFrom = ref<String[]>([]);
    const chosenNames = ref<String[]>([]);
    const list: Ref<HTMLElement | null> = ref(null);
    const isActiveSelectAll = ref<Boolean>(false);

    provide("namesToChooseFrom", namesToChooseFrom);

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

    const toggleSelectAll = () => {
      isActiveSelectAll.value = isActiveSelectAll.value ? false : true;

      if (isActiveSelectAll.value) {
        namesToChooseFrom.value = names.value;
      } else {
        namesToChooseFrom.value = [];
      }
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
      chosenNames,
      addName,
      removeName,
      list,
      toggleSelectAll,
    };
  },
};
</script>

<style scoped lang="scss">
.user-names {
  width: 50%;
}

.container {
  display: flex;
  width: 100%;
  height: 100%;
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
