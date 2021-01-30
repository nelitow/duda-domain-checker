<template>
  <div class="container">
    <DomainChecker />
    <h3>Other information</h3>
    <div class="card">
      You can find full instructions
      <a
        href="https://support.duda.co/hc/en-us/articles/360042435774-Go-Live-Publish-and-Set-Up-Your-Domain"
      >
        here.
      </a>
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

<style scoped lang="scss">
body {
  font-family: "Montserrat", sans-serif;
  background: #ebeae7;
  color: #4a4a4a;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  .card {
    padding: 16px;
    width: 100%;
    margin: 0 0;
    background-color: #fff;
    border-radius: 8px;
    box-shadow: rgba(0, 0, 0, 0.06) 0px 2px 8px,
      rgba(0, 0, 0, 0.05) 0px 0.5px 1px;
  }
  button {
    border-radius: 1234px;
    border: none;
    padding: 6px 18px;
    color: #fff;
    background-color: #f66035;
    font-size: 12px;
    margin: 0 6px;
    transition: all 0.2s ease;
  }
  h3 {
    font-size: 14px;
    font-weight: 600;
    text-transform: uppercase;
    margin: 10px 0;
  }
}
</style>
