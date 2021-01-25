<template>
  <div class="domainChecker">
    <div class="content">
      <input
        v-model="domain"
        type="text"
        class="domain"
        placeholder="Enter your domain here"
      />
      <Registrar :domain="hostnameRoot" />
      <table class="table">
        <tr>
          <td>{{ hostname }}</td>
          <td>
            <ul>
              <li v-for="(record, index) in records" :key="index">
                <span
                  class="green"
                  v-if="
                    [
                      '35.172.94.1',
                      '100.24.208.97',
                      's.multiscreensite.com.',
                    ].includes(record.data)
                  "
                  >{{ record.data }}</span
                >
                <span v-else class="red">{{ record.data }}</span>
              </li>
            </ul>
          </td>
        </tr>
        <tr class="hr">
          <td>{{ hostnameRoot }}</td>
          <td>
            <ul>
              <li v-for="(record, index) in recordsRoot" :key="index">
                <span
                  class="green"
                  v-if="
                    [
                      '35.172.94.1',
                      '100.24.208.97',
                      's.multiscreensite.com.',
                    ].includes(record.data)
                  "
                  >{{ record.data }}</span
                >
                <span v-else class="red">{{ record.data }}</span>
              </li>
            </ul>
          </td>
        </tr>
        <tr v-if="caa" class="hr">
          <td>CAA</td>
          <td>
            <span class="red"
              >CAA Record detected! Please remove to generate SSL
              certificate.</span
            >
          </td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
import Registrar from "../components/Registrar.vue";

export default {
  name: "DomainChecker",
  components: {
    Registrar,
  },
  data() {
    return {
      domain: "",
      records: {},
      recordsRoot: {},
      caa: {},
      ns: {},
    };
  },
  created() {
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
  },
  methods: {
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
        });
    },
  }, 
};
</script>

<style scoped lang="scss">
.red {
  color: #a44;
}
.green {
  color: #4a4;
}

.table {
  width: 100%;
  margin: 32px 0;
  background-color: #fff;
  border-radius: 8px;
  box-shadow: rgba(0, 0, 0, 0.06) 0px 2px 8px, rgba(0, 0, 0, 0.05) 0px 0.5px 1px;
  thead {
  }
  td,
  th {
    padding: 16px;
  }
  tr {
    &.hr {
      border-top: 1px solid #00000013;
    }
  }
}

input.domain {
  border-radius: 8px;
  border: 1px solid grey;
  padding: 8px 16px;
  margin: 16px 0;
  min-width: 256px;
  background-image: url("../assets/002-sphere.svg");
  background-size: 24px auto;
  background-repeat: no-repeat;
  background-position: 96% center;
  font-size: 16px;
}
</style>
