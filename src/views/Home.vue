<template>
  <div class="home">
    <img class="logo" alt="Vue logo" src="../assets/d&d.png" />
    <HelloWorld />
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";

export default {
  name: "Home",
  components: {
    HelloWorld,
  },
};
</script>

<style>
.logo {
  width: 160px;
  height: 100px;
}
</style>
