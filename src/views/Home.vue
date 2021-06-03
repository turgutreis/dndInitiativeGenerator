<template>
  <div class="home">
    <img class="logo" alt="Vue logo" src="../assets/d&d.png" />
    <InitiativeGenerator />
  </div>
</template>

<script>
// @ is an alias to /src
import InitiativeGenerator from "@/components/InitiativeGenerator.vue";

export default {
  name: "Home",
  components: {
    InitiativeGenerator,
  },
};
</script>

<style>
.logo {
  width: 160px;
  height: 100px;
}
</style>
