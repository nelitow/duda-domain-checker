<template>
  <div class="container">
    <DomainChecker />
    <div class="content">
      <p>
        You can find full instructions
        <a
          href="https://support.duda.co/hc/en-us/articles/360042435774-Go-Live-Publish-and-Set-Up-Your-Domain"
        >
          here.</a
        >
      </p>
    </div>
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
body {
  font-family: "Montserrat", sans-serif;
  background: #ebeae7;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
}
</style>
