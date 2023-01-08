<template>
  <v-dialog v-model="proxyDialogDelete" max-width="500px"> </v-dialog>
</template>
<script>
import Vue from "vue";

import cors from "cors";
Vue.use(cors);


export default {
  name: "DeleteItem",
  props: [
    "dialogDelete",
    "editedIndex",
    "editedItem",
    "defaultItem",
    "item",
    "farms",
  ],
  data() {
    return {
      proxyDialogDelete: false,
      dBook: this.item,
    };
  },
  created() {
    this.proxyEditedItem = this.editedItem;
    this.proxyEditedIndex = this.editedIndex;
    this.proxyDialogDelete = this.dialogDelete;
  },
  methods: {
    async deleteItem(item) {
     
      fetch(
        `https://lugg9ewkq6.execute-api.us-east-1.amazonaws.com/Test/farm/${item.farm_id}`,
        {
          method: "Delete",
          headers: {
            "Content-Type": "application/json",
          },
        }
      ).then((res)=>{
        console.log("on delete")
      }).catch((err)=>{
        window.location.reload();
      });
    },
  },
};
</script>
