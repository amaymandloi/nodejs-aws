<template>
  <v-toolbar>
    <v-toolbar-title>Farm_Details</v-toolbar-title>
    <v-spacer></v-spacer>
    <v-dialog max-width="600px" v-model="dialog">
      <template v-slot:activator="{ on, attrs }">
        <v-btn color="blue lighten-2" v-bind="attrs" v-on="on">NewFarm</v-btn>
      </template>
      <v-card>
        <v-card-title class="pa-3 ma-3"><h2>new farm</h2></v-card-title>
        <v-text-field label="Farm Id" v-model="newFarm.farm_id" class="pa-3 ma-3">
        </v-text-field>
        <v-text-field label="Farm Name" v-model="newFarm.farm_name" class="pa-3 ma-3">
        </v-text-field>
        <v-text-field label="City" v-model="newFarm.city" class="pa-3 ma-3">
        </v-text-field>
        <v-text-field label="lat" v-model="newFarm.lat" class="pa-3 ma-3">
        </v-text-field>
        <v-text-field label="long" v-model="newFarm.long" class="pa-3 ma-3">
        </v-text-field>
        <v-text-field label="country" v-model="newFarm.county" class="pa-3 ma-3">
        </v-text-field>

        <v-card-actions>
          <v-btn color="blue" v-on:click="addItem">SAVE</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-toolbar>
</template>
<script>
import Vue from "vue";
import cors from "cors";
Vue.use(cors);

export default {
  name: "Dialog",
  data() {
    return {
      newFarm: {
        farm_id: "",
        farm_name: "",
        city: "",
        lat: "",
        long: "",
        county: "",
      },

      dialog: false,
      menu: false,
    };
  },
  watch: {
    menu(val) {
      val;
    },
  },
  methods: {
    async addItem() {
      if (
        !this.newFarm.farm_name ||
        !this.newFarm.farm_id ||
        !this.newFarm.city ||
        !this.newFarm.lat ||
        !this.newFarm.long ||
        !this.newFarm.county
      ) {
        alert("Null entry cannot be added!");
      } else {
        const res = await fetch(
          "https://lugg9ewkq6.execute-api.us-east-1.amazonaws.com/Test/farm",
          {
            method: "POST",
            headers: {
              accept: "application/json",

              "Content-Type": "application/json",
            },
            body: JSON.stringify(this.newFarm),
          }
        );
      }
    },
  },
};
</script>
