<template>
  <div class="card">
    <div class="frame">
      <iframe :src="`https://www.whatsmydns.net/#A/${domain}`"></iframe>
    </div>
    <div class="frame">
      <iframe :src="`https://www.whatsmydns.net/#CNAME/www.${domain}`"></iframe>
    </div>
  </div>
</template>

<script>
export default {
  name: "Registrar",
  data() {
    return {};
  },
  props: ["domain"],
  watch: {},
  computed: {},
  created() {},
  methods: {},
};
</script>

<style scoped lang="scss">
.card {
  grid-template-columns: auto auto;
  grid-gap: 16px;
  place-content: stretch;
}
@media screen and (max-width: 780px) {
  .card {
    grid-template-columns: 1fr;
  }
}
.frame {
  width: 100%;
  display: inline-block;
  border-radius: 8px;
  overflow: hidden;
  height: 50vh;
}
iframe {
  width: 200%;
  border: none;
  height: 100vh;
  transform: scale(0.5);
  transform-origin: 0 0;
}
</style>
