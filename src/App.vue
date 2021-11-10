<template>
  <v-app>
    <h1>QRCode Generator</h1>
    <code-generator />
  </v-app>
</template>

<script>
import CodeGenerator from "./components/CodeGenerator.vue";
export default {
  name: "App",
  components: {
    CodeGenerator,
  },
};
</script>

<style lang="scss">
.v-application--wrap {
  padding: 30px 15px;
  max-width: 1000px !important;
  margin: 0 auto;
}
html {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
body {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  color: #2c3e50;
}
h1,
h2,
h3,
h4,
h5,
h6 {
  margin: 0 0 10px;
}
p {
  margin: 0 0 5px;
}
ul,
li {
  padding: 0;
  list-style: none;
}
.v-application {
  ul {
    padding: 0;
  }
}
h1 {
  text-align: center;
}
</style>
