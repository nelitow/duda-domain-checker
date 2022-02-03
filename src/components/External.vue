<template>
  <div class="card">
    <div class="button">
      <a :href="`https://www.whatsmydns.net/#A/${domain}`">Whats My DNS</a>
      <a :href="`https://www.whatsmydns.net/#CNAME/www.${domain}`"
        >DNS Checker</a
      >
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

.button a {
  border-radius: 1234px;
  border: none;
  padding: 16px 18px;
  color: #fff;
  text-decoration: none;
  background-color: #f66035;
  font-size: 12px;
  margin: 4px;
  transition: all 0.2s ease;
  height: 38px;
}

</style>
