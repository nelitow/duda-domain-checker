<template>
  <div class="domainChecker">
    <div class="head">
      <span>
        {{ hostname }}
      </span>
    </div>
    <div class="content">
      <h3>Domain:</h3>
      <input v-model="domain" type="text" class="input shadow" />

      <table class="shadow">
        <thead>
          <th>Domain</th>
          <th>Record</th>
        </thead>
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
        <tr>
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
        <tr v-if="caa">
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
export default {
  name: "DomainChecker",
  data() {
    return {
      domain: "https://www.duda.life",
      records: {},
      recordsRoot: {},
      caa: {},
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
        return "Invalid domain";
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
  color: #f25340;
}

.green {
  color: #7ca783;
}

.head {
  background-color: orange;
  padding: 20px;
  color: #fff;
  text-transform: uppercase;
  font-weight: bold;
}

.shadow {
  box-shadow: 0 2px 4px 1px #0001;
}

input {
  border: none;
  padding: 10px 15px 10px 40px;
  border-radius: 10px;
  margin-bottom: 20px;
  background-image: linear-gradient(#fffc, #fffc),
    url("https://img.freepik.com/free-icon/world-wide-web_318-9868.jpg?size=338&ext=jpg");
  background-size: 20px;
  background-repeat: no-repeat;
  background-position-x: 10px;
  background-position-y: center;
}

table {
  background: white;
  padding: 10px 15px 10px 40px;
  border-radius: 10px;
  margin-bottom: 20px;
}
</style>
