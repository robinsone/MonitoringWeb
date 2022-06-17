<template>
  <div class="container">
    <div class="row">
      <div class="col">
        <v-card>
          <v-card-title>
            <v-text-field
              v-model="search"
              append-icon="mdi-magnify"
              label="Search"
              single-line
              hide-details
            ></v-text-field>
          </v-card-title>
          <v-data-table :headers="headers" :items="stats" :search="search">
            <template v-slot:[`item.actions`]="{ item }">
              <v-icon class="mr-2" @click="getStats(item)">
                mdi-download
              </v-icon>
            </template>
          </v-data-table>
        </v-card>
      </div>
    </div>
  </div>
</template>

<script>
import vessels from "../../vessels.js";

export default {
  name: "HelloWorld",

  data: () => ({
    headers: [
      { text: "Vessel", value: "name", align: "start" },
      { text: "OS", value: "operatingSystem" },
      { text: "Version", value: "operatingSystemVersion" },
      { text: "Uptime", value: "uptime" },
      { text: "Remaining Disk Space", value: "memUsage" },
      { text: "CPU Usage", value: "cpuUsage" },
      { text: "Ram Usage", value: "ramUsage" },
      { text: "Actions", value: "actions", sortable: false },
    ],
    stats: [],
    search: "",
    baseCheckURI: "/api/check",
  }),

  computed: {},

  methods: {
    async getStats(vessel) {
      const baseURI = vessel.url + this.baseCheckURI;
      this.$http.get(baseURI).then((result) => {
        result.data.name = vessel.name;
        result.data.url = vessel.url;
        var existing = this.stats.findIndex(
          (stat) => stat.name == result.data.name
        );
        if (existing >= 0) {
          this.stats = this.stats.map((s) =>
            s.name == result.data.name ? { ...result.data } : s
          );
        } else {
          this.stats = this.stats.concat(result.data);
        }
      });
    },
  },

  mounted() {
    this.stats = vessels;
  },
};
</script>
