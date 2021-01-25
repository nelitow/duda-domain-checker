<template>
  <div class="registrar">
    <div class="content">
      Your domain is managed at:
      <a target="blank" :href="`https://${registrar}`">{{ registrar }}</a>
    </div>
  </div>
</template>

<script>
export default {
  name: "Registrar",
  data() {
    return {
      ns: [{ data: "empty" }],
    };
  },
  props: ["domain"],
  watch: {
    domain() {
      this.fetchDomain();
    },
  },
  computed: {
    registrar() {
      const registrarUrl = this.ns[0].data;
      if (registrarUrl.includes("cloudflare")) return "cloudflare.com";
      if (registrarUrl.includes("registrar-servers.com"))
        return "namecheap.com";
      if (registrarUrl.includes("domaincontrol.com"))
        return "godaddy.com";

      return registrarUrl;
    },
  },
  created() {
    this.fetchDomain();
  },
  methods: {
    fetchDomain() {
      const ns = `https://cloudflare-dns.com/dns-query?name=${this.domain}&type=NS`;

      fetch(ns, {
        method: "get",
        headers: {
          "Content-Type": "application/json",
          Accept: "application/dns-json",
        },
      })
        .then((r) => r.json())
        .then((r) => {
          console.log(r);
          if (r.Answer) this.ns = r.Answer;
        });
    },
  },
};
</script>

<style scoped lang="scss">
</style>
