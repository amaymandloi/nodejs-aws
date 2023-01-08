<template>
  <v-dialog v-model="proxyDialog" max-width="500px">
    <v-card>
      <v-card-title>
        <span class="text-h5">{{ formTitle }}</span>
      </v-card-title>
      <v-card-text>
        <v-container>
          <v-row>
            <v-col cols="12" sm="6" md="4">
              <v-text-field
                v-model="proxyEditedItem.farm_id"
                label="Farm ID"
              ></v-text-field>
            </v-col>
            <v-col cols="12" sm="6" md="4">
              <v-text-field
                v-model="proxyEditedItem.farm_name"
                label="Farm Name"
              ></v-text-field>
            </v-col>
            <v-col cols="12" sm="6" md="4">
              <v-text-field
                v-model="proxyEditedItem.city"
                label="City"
              ></v-text-field>
            </v-col>
            <v-col cols="12" sm="6" md="4">
              <v-text-field
                v-model="proxyEditedItem.lat"
                label="lat"
              ></v-text-field>
            </v-col>
            <v-col cols="12" sm="6" md="4">
              <v-text-field
                v-model="proxyEditedItem.long"
                label="long"
              ></v-text-field>
            </v-col>
            <v-col cols="12" sm="6" md="4">
              <v-text-field
                v-model="proxyEditedItem.county"
                label="country"
              ></v-text-field>
            </v-col>
          </v-row>
        </v-container>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="blue darken-1" text @click="close"> Cancel </v-btn>
        <v-btn color="blue darken-1" text @click="save"> Save </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>
<script>
import Vue from "vue";
import cors from "cors";
Vue.use(cors);

export default {
  name: "EditItem",
  props: [
    "editedIndex",
    "defaultItem",
    "item",
    "dialog",
    "editedItem",
    "farms",
  ],
  data() {
    return {
      proxyEditedItem: {
        farm_id: "",
        farm_name: "",
        cit: "",
        lat: "",
        long: "",
        country: "",
      },
      proxyEditedIndex: -1,
    };
  },
  created() {
    this.proxyEditedItem = this.editedItem;
    this.proxyEditedIndex = this.editedIndex;
    this.proxyDialog = this.dialog;
  },
  methods: {
    editTableItem(proxyEditedItem, proxyEditedIndex, proxyDialog) {
      this.$emit("sendData", proxyEditedItem, proxyEditedIndex, proxyDialog);
    },
    editItem(item) {
      this.proxyEditedIndex = this.farms.indexOf(item);
      this.proxyEditedItem = Object.assign({}, item);
      this.proxyDialog = true;
    },
    async save() {
      const editItemData = this.proxyEditedItem;
      console.log(editItemData)
      const res = await fetch(
        "https://lugg9ewkq6.execute-api.us-east-1.amazonaws.com/Test/farm",
        {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify(editItemData),
        }
      );
      const data = await res.json();
      this.close();
    },
    close() {
      this.proxyDialog = false;
      this.$nextTick(() => {
        this.proxyEditedItem = Object.assign({}, this.defaultItem);
        this.proxyEditedIndex = -1;
      });
    },
  },
  watch: {
    dialog(val) {
      val || this.close();
    },
  },
  computed: {
    formTitle() {
      return this.proxyEditedIndex === -1 ? "New Item" : "Edit Item";
    },
  },
};
</script>
