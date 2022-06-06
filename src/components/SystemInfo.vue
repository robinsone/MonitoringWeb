<template>
  <div class="container">
    <div class="row">
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
        <v-data-table
          :headers="headers"
          :items="stats"
          :search="search"
        ></v-data-table>
      </v-card>
    </div>
  </div>
</template>

<script>
import vessels from "../../vessels.js";

export default {
  name: "HelloWorld",

  data: () => ({
    headers: [
      { text: "Vessel", value: "hostName", align: "start" },
      { text: "OS", value: "operatingSystem" },
      { text: "Version", value: "operatingSystemVersion" },
      { text: "Uptime", value: "uptime" },
      { text: "Memory Usage", value: "memUsage" },
      { text: "CPU Usage", value: "cpuUsage" },
      { text: "Ram Usage", value: "ramUsage" },
      { text: "Actions", value: "" },
    ],
    stats: [],
    search: "",
  }),

  computed: {},

  methods: {
    getStats(item) {
      const baseURI = item;
      this.$http.get(baseURI).then((result) => {
        console.log(result.data);
        this.stats = this.stats.concat(result.data);
      });
    },
  },

  mounted() {
    const baseURI = "/api/check";
    this.getStats(vessels.test + baseURI);
  },
};
</script>
