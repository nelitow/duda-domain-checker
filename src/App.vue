<template>
  <div class="container">
    <DomainChecker />
  </div>
</template>

<script>
import DomainChecker from "./components/DomainChecker.vue";

export default {
  name: "App",
  components: {
    DomainChecker,
  },
};
</script>

<style lang="scss">
.container {
  display: grid;
  min-height: 600px;
  grid-gap: 16px;
  margin: 16px auto;
  max-width: 1366px;
  padding: 0 16px;
  input {
    border: 1px solid #ccc;
  }
}
.card {
  margin: 0;
  padding: 16px;
  background-color: #fff;
  box-shadow: rgba(0, 0, 0, 0.06) 0px 2px 8px, rgba(0, 0, 0, 0.05) 0px 0.5px 1px;
  border-radius: 8px;
  display: grid;
  place-content: center left;
}
h3 {
  display: grid;
  margin: 0;
  place-content: end left;
}
body {
  margin: 0;
  font-family: "Montserrat", sans-serif;
  background: #ebeae7;
  color: #4a4a4a;
}
</style>
