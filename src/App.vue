<template>
  <div class="container">
    <DomainChecker />
    <div class="content">
      <!-- #######  YAY, I AM THE SOURCE EDITOR! #########-->
      <p>Use the following information for the CNAME record and A records:</p>
      <h4>CNAME</h4>
      <ul>
        <li value="1"><em>Host</em>.&nbsp;www</li>
        <li value="2"><em>Points to</em>.&nbsp;s.multiscreensite.com</li>
      </ul>
      <h4>A Records</h4>
      <ul>
        <li value="1">
          <strong>First A Record:</strong>
          <ul>
            <li value="1">Host/Alias.&nbsp;@</li>
            <li value="2">Target/Destination.&nbsp;35.172.94.1</li>
          </ul>
        </li>
        <li value="2">
          <strong>Second A Record:</strong>
          <ul>
            <li value="1"><em>Host/Alias</em>@</li>
            <li value="2"><em>Target/Destination</em>.&nbsp;100.24.208.97</li>
          </ul>
        </li>
      </ul>
      <p>Two A Records are used for server balance and redundancy.</p>
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
  background: url("https://source.unsplash.com/random");
  background-size: cover;
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  min-height: 100vh;
  margin: 0;
}
html {
  margin: 0;
  padding: 0;
}

.container {
  overflow: hidden;
  max-width: 800px;
  width: 100%;
  margin: 0 auto;
  padding: 0;
  backdrop-filter: blur(10px);
  background-color: #fff4;
  border-radius: 10px;
  box-shadow: 0 2px 4px 1px #0005;

  .content {
    padding: 20px;
  }
}
</style>
