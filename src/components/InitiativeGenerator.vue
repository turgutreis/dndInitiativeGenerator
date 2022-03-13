<template>
  <div>
    <div class="card-form">
      <form @submit.prevent="addNewPlayer">
        <label>Playername</label>
        <input v-model="playerName" name="playerName" />
        <label>Ini Modifer</label>
        <input v-model="iniModifier" name="iniModifier" />
        <!-- <input type="radio" id="advantage" name="status" value="advantage" /> -->
        <!-- <label for="advantage">Advantage</label><br /> -->
        <button class="btn success mt-2">Add New Player</button>
      </form>
      <form action="" @submit.prevent="addNumberOfEnemies">
        <label>Enemy Name</label>
        <input v-model="enemiesName" name="enemiesName" />
        <label>Ini Modifer</label>
        <input v-model="iniModifierEnemies" name="iniModifierEnemies" />
        <label>number of Enemies</label>
        <input v-model="enemiesNumber" name="enemiesNumber" />
        <button class="btn success mt-2">Add Enemies</button>
      </form>
      <button class="btn success mt-2" @click="rollAllDices">
        Roll all dices
      </button>
      <button class="btn danger mt-2" @click="sortPlayers">SORT</button>
    </div>
    <ol>
      <div class="card" v-for="data in players" :key="data.id">
        <li>
          <h3>Playername: {{ data.playername }}</h3>
          <h3>Initiative Modifier: {{ data.modifier }}</h3>
          <h3 v-if="data.diceVal > 0">Initiative: {{ data.diceVal }}</h3>
          <button
            class="btn success"
            @click="diceGenerator(data.modifier, data.id)"
          >
            Roll the Dice
          </button>
          <button class="btn warning" @click="removePlayer(data.id)">
            Delete Player
          </button>
        </li>
      </div>
    </ol>
  </div>
</template>

<script>
import { ref } from "vue";
import { nanoid } from "nanoid";
export default {
  name: "InitiativeGenerator",
  mounted() {
    if (localStorage.getItem("player")) {
      try {
        this.players = JSON.parse(localStorage.getItem("player"));
        this.enemies = JSON.parse(localStorage.getItem("enemies"));
      } catch (e) {
        localStorage.removeItem("player");
      }
    }
  },
  setup() {
    const playerName = ref("");
    const enemiesName = ref("");
    const enemiesNumber = ref("");
    const iniModifier = ref("");
    const iniModifierEnemies = ref("");
    const players = ref([]);
    const enemies = ref([]);
    const playersAndEnemies = ref([]);
    function addNewPlayer() {
      players.value.push({
        id: nanoid(6),
        playername: playerName.value,
        modifier: iniModifier.value,
        diceVal: 0,
      });
      const parsed = JSON.stringify(players.value);
      localStorage.setItem("player", parsed);
      console.log(players.value);
      playerName.value = "";
      iniModifier.value = "";
    }
    function addNumberOfEnemies() {
      for (let i = 0; i < enemiesNumber.value; i++) {
        players.value.push({
          id: nanoid(6),
          playername: enemiesName.value + " " + i,
          modifier: iniModifierEnemies.value,
          diceVal: 0,
        });
      }
      const parsed = JSON.stringify(players.value);
      localStorage.setItem("player", parsed);
      console.log(players.value);
      playerName.value = "";
      iniModifier.value = "";
    }
    function sortPlayers() {
      players.value.sort((a, b) => b.diceVal - a.diceVal);
      const value = JSON.parse(localStorage.getItem("player"));
      var newVal = value.sort((a, b) => b.diceVal - a.diceVal);
      const parsed = JSON.stringify(newVal);
      localStorage.setItem("player", parsed);
    }
    function diceGenerator(modifier, id) {
      var result = Math.floor(Math.random() * 20) + 1;
      console.log(result + parseInt(modifier));
      players.value.filter((resp) => {
        if (resp.id === id) {
          resp.diceVal = result + parseInt(modifier);
        }
      });
      const parsed = JSON.stringify(players.value);
      localStorage.setItem("player", parsed);
    }
    function rollAllDices() {
      var result = 0;
      for (let i = 0; i < players.value.length; i++) {
        result = Math.floor(Math.random() * 20) + 1;
        players.value[i].diceVal = result + parseInt(players.value[i].modifier);
      }
      const parsed = JSON.stringify(players.value);
      localStorage.setItem("player", parsed);
    }
    function removePlayer(val) {
      const index = players.value.map((item) => item.id).indexOf(val);
      players.value.splice(index, 1);
      const parsed = JSON.stringify(players.value);
      localStorage.setItem("player", parsed);
    }

    return {
      players,
      enemies,
      playersAndEnemies,
      playerName,
      enemiesName,
      iniModifier,
      addNewPlayer,
      enemiesNumber,
      diceGenerator,
      iniModifierEnemies,
      rollAllDices,
      sortPlayers,
      removePlayer,
      addNumberOfEnemies,
    };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
* {
  box-sizing: border-box;
}
.card {
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  margin: auto;
  margin-top: 10px;
  padding: 16px;
  width: 30vh;
  text-align: center;
  background-color: #f1f1f1;
}
.card-form {
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  margin: auto;
  margin-top: 10px;
  padding: 16px;
  width: 235px;
  text-align: center;
  background-color: #f1f1f1;
}
.warning {
  background-color: #ff9800;
  font-weight: bold;
  color: #f1f1f1;
} /* Orange */
.warning:hover {
  background: #e68a00;
}
.success {
  font-weight: bold;
  background-color: #04aa6d;
  color: #f1f1f1;
} /* Green */
.success:hover {
  background-color: #46a049;
}
.danger {
  font-weight: bold;
  background-color: #f44336;
  color: #f1f1f1;
} /* Red */
.danger:hover {
  background: #ff1100;
}
</style>
