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
    <button @click="sortPlayers">sort</button>
    <ol>
      <li v-for="data in players" :key="data.id">
        <h3>Playername: {{ data.playername }}</h3>
        <h3>Initiative Modifier: {{ data.modifier }}</h3>
        <h3 v-if="data.diceVal > 0">Initiative: {{ data.diceVal }}</h3>
        <button @click="diceGenerator(data.modifier, data.id)">
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
    const players = ref([]);
    function addNewPlayer() {
      players.value.push({
        id: playerId.value++,
        playername: playerName.value,
        modifier: iniModifier.value,
        diceVal: 0,
      });
      playerName.value = "";
      iniModifier.value = "";
    }
    function sortPlayers() {
      players.value.sort((a, b) => b.diceVal - a.diceVal);
    }
    function diceGenerator(modifier, id) {
      var result = Math.floor(Math.random() * 20) + 1;
      console.log(result + parseInt(modifier));
      players.value.filter((resp) => {
        if (resp.id === id) {
          resp.diceVal = result + parseInt(modifier);
        }
      });
      // console.log(todos.value.sort((a, b) => a - b));
    }
    function rollAllDices() {
      var result = 0;
      for (let i = 0; i < players.value.length; i++) {
        result = Math.floor(Math.random() * 20) + 1;
        players.value[i].diceVal = result + parseInt(players.value[i].modifier);
      }
    }
    return {
      players,
      playerName,
      iniModifier,
      addNewPlayer,
      diceGenerator,
      rollAllDices,
      sortPlayers,
    };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
