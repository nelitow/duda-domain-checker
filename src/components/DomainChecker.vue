<template>
  <div class="form">
    <input
      v-model="domain"
      type="text"
      class="domain card"
      placeholder="Enter your domain here"
      @keypress.enter="reloadFrames"
    />
    <button @click="reloadFrames">Check</button>
  </div>
  <Registrar :domain="hostnameRoot" />
  <h3>Records</h3>
  <div class="card records">
    <div>
      <b>{{ hostnameRoot }}</b>
      <div v-for="(record, index) in recordsRoot" :key="index">
        <span
          class="green"
          v-if="
            ['35.172.94.1', '100.24.208.97', 's.multiscreensite.com.'].includes(
              record.data
            )
          "
        >
          ✅ {{ record.data }}<br />
        </span>
        <span v-else class="red">
          ⚠️ {{ record.data }} This record does not point to Duda
        </span>
      </div>
    </div>
    <div>
      <b>{{ hostname }}</b>
      <div v-for="(record, index) in records" :key="index">
        <span
          class="green"
          v-if="
            ['35.172.94.1', '100.24.208.97', 's.multiscreensite.com.'].includes(
              record.data
            )
          "
        >
          ✅ {{ record.data }}<br />
        </span>
        <span v-else class="red">
          ⚠️ {{ record.data }} This record does not point to Duda
        </span>
      </div>
    </div>
    <div v-if="caa" class="hr">
      <span class="red">
        ⚠️ CAA Record detected! Please remove to generate SSL certificate.
      </span>
    </div>
    <div v-if="!allCorrect" class="hr">
      <span class="red">
        Please note: if any change has been made recently, it can take a few
        minutes or up to 48 hours for the changes to be applied. <br />In some
        cases results may be cached for people in different parts of the world,
        this may mean that some people will be receiving old results until the
        domain cache expires.
      </span>
    </div>
  </div>
  <h3>Other tools</h3>

  <External :domain="hostnameRoot" />
</template>

<script>
import Registrar from "../components/Registrar.vue";
import External from "../components/External.vue";

export default {
  name: "DomainChecker",
  components: {
    Registrar,
    External,
  },
  data() {
    return {
      domain: "",
      records: [],
      recordsRoot: [],
      caa: false,
      ns: {},
    };
  },
  created() {
    let urlParams = new URLSearchParams(window.location.search);
    if (urlParams.has("url")) this.domain = urlParams.get("url");
    this.fetchDomain();
  },
  watch: {
    domain() {
      this.fetchDomain();
    },
  },
  computed: {
    hostname() {
      if (this.domain.includes("http") && this.domain.length > 10) {
        const url = new URL(this.domain);
        return url.hostname;
      } else {
        return this.domain;
      }
    },
    hostnameRoot() {
      return this.hostname.replace("www.", "");
    },
    allCorrect() {
      let c = true;

      this.records.concat(this.recordsRoot).forEach(function (val) {
        if (
          !["35.172.94.1", "100.24.208.97", "s.multiscreensite.com."].includes(
            val.data
          )
        ) {
          console.log([val.data]);
          c = false;
        }
      });

      return c;
    },
  },
  methods: {
    reloadFrames() {
      var url = new URL(window.location);
      url.searchParams.has("url")
        ? url.searchParams.set("url", this.domain)
        : url.searchParams.append("url", this.domain);
      console.log(url);
      window.location.href = url;
    },
    fetchDomain() {
      const full = `https://cloudflare-dns.com/dns-query?name=${this.hostname}&type=A`;

      fetch(full, {
        method: "get",
        headers: {
          "Content-Type": "application/json",
          Accept: "application/dns-json",
        },
      })
        .then((r) => r.json())
        .then((r) => {
          this.records = r.Answer;
        });
      const root = `https://cloudflare-dns.com/dns-query?name=${this.hostnameRoot}&type=A`;
      fetch(root, {
        method: "get",
        headers: {
          "Content-Type": "application/json",
          Accept: "application/dns-json",
        },
      })
        .then((r) => r.json())
        .then((r) => {
          this.recordsRoot = r.Answer;
        });
      const caa = `https://cloudflare-dns.com/dns-query?name=${this.hostnameRoot}&type=CAA`;
      fetch(caa, {
        method: "get",
        headers: {
          "Content-Type": "application/json",
          Accept: "application/dns-json",
        },
      })
        .then((r) => r.json())
        .then((r) => {
          this.caa = r.Answer;
          this.allCorrect = false;
        });
    },
  },
};
</script>

<style scoped lang="scss">
.records {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  grid-gap: 16px;
  > div {
    border-left: 1px solid #ebeae7;
    padding-left: 8px;
  }
}
.form {
  display: flex;
  flex-wrap: wrap;
  input {
    flex: 10;
    height: 5px;
  }
  button {
    flex: 2;
    margin-left: 8px;
  }
}
</style>
