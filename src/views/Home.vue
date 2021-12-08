<template>
  <div class="home">
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
