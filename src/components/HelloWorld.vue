<template>
  <div>
    <h1>Vue 3 Initiative Generator D&D 5</h1>
    <form @submit.prevent="addNewPlayer">
      <label>Name</label>
      <input v-model="playerName" name="playerName" />
      <label>Ini Modifer</label>
      <input v-model="iniModifier" name="iniModifier" />
      <button>Add New Player</button>
    </form>
    <button class="btn btn-primary" @click="rollAllDices">
      Roll all dices
    </button>
    <ol>
      <li v-for="todo in todos" :key="todo.id">
        <h3>Playername: {{ todo.playername }}</h3>
        <h3>Initiative Modifier: {{ todo.modifier }}</h3>
        <h3 v-if="todo.diceVal > 0">Initiative: {{ todo.diceVal }}</h3>
        <button @click="diceGenerator(todo.modifier, todo.id)">
          Roll the Dice
        </button>
      </li>
    </ol>
  </div>
</template>

<script>
import { ref } from "vue";
export default {
  name: "HelloWorld",
  setup() {
    const playerName = ref("");
    const iniModifier = ref("");
    const playerId = ref("1");
    const todos = ref([]);
    function addNewPlayer() {
      todos.value.push({
        id: playerId.value++,
        playername: playerName.value,
        modifier: iniModifier.value,
        diceVal: 0,
      });
      playerName.value = "";
      iniModifier.value = "";
    }
    function diceGenerator(modifier, id) {
      var result = Math.floor(Math.random() * 20) + 1;
      console.log(result + parseInt(modifier));
      todos.value.filter((resp) => {
        if (resp.id === id) {
          resp.diceVal = result + parseInt(modifier);
        }
      });
    }
    function rollAllDices() {
      var result = 0;
      for (let i = 0; i < todos.value.length; i++) {
        result = Math.floor(Math.random() * 20) + 1;
        console.log(result);
        todos.value[i].diceVal = result + parseInt(todos.value[i].modifier);
      }
    }
    return {
      todos,
      playerName,
      iniModifier,
      addNewPlayer,
      diceGenerator,
      rollAllDices,
    };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
