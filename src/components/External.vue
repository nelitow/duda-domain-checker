<template>
  <div class="registrar card">
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
.frame {
  width: 50%;
  display: inline-block;
  border-radius: 8px;
  overflow: hidden;
  height: 400px;
}
iframe {
  width: 200%;
  border: none;
  height: 800px;
  transform: scale(0.5);
  transform-origin: 0 0;
}
</style>
