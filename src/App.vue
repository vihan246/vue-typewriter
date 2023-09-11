<template>
  <div class="demo-container">
    <TypeWriter class="demo" :text="listOfStrings" delete="type" repeats />
  </div>
</template>

<script>
import TypeWriter from "./components/TypeWriter.vue";

export default {
  name: "App",
  components: { TypeWriter },
  setup() {
    const listOfStrings = ["Demo text", "Vue component", "abcdef"];
    return {
      listOfStrings,
    };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.demo-container {
  width: 100%;
  height: 400px;
}
.demo {
  font-size: large;
  width: 100px;
  height: 100px;
}
</style>
