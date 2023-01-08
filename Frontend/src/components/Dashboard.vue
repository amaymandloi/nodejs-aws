<template>
  <div>
    <Dialog v-on:addItem="updateTable($event)" />
    <TableList
      v-bind:headers="headers"
      v-bind:farms="farms"
      v-bind:dialog="dialog"
      v-bind:dialogDelete="dialogDelete"/>
  </div>
</template>
<script>
import Dialog from "../components/Dialog.vue";
import TableList from "../components/TableList.vue";
import Vue from "vue";
import cors from "cors";

Vue.use(cors);

export default {
  name: "Dashboard",
  data() {
    return {
      newBook: {},

      headers: [
        {
          text: "Farm ID",
          value: "farm_id",
          align: "start",
          sortable: false,
        },
        {
          text: "Farm Name",
          value: "farm_name",
        },
        {
          text: "City",
          value: "city",
        },
        {
          text: "Country",
          value: "county",
        },
        {
          text: "Lat",
          value: "lat",
        },
        {
          text: "Long",
          value: "long",
        },
       
        {
          text: "Edit",
          value: "edit",
          sortable: false,
        },
        {
          text: "Delete",
          value: "delete",
          sortable: false,
        },
      ],

      farms: [],
      dialog: false,
      dialogDelete: false,
      editedIndex: -1,
      editedItem: {
        farm_id: "",
        farm_name: "",
        city: "",
        county: "",
        lat: "",
        long: "",
        
      },
      defaultItem: {
        farm_id: "",
        farm_name: "",
        city: "",
        county: "",
        lat: "",
        long: "",
        
      },
    };
  },
  components: {
    Dialog,
    TableList,
  },

  async created() {
    const res = await fetch(
      "https://lugg9ewkq6.execute-api.us-east-1.amazonaws.com/Test/farm",
      {
        method: "GET",
        headers: {
          accept: "application/json",
        },
      }
    );

    const farmsObj = await res.json();
    const arr = [];
    farmsObj.Items.map((element) => arr.push(element));
    const farmsData = JSON.parse(JSON.stringify(arr));
    this.farms = farmsData;
  },
};
</script>
