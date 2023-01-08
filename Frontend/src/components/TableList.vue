<template>
  <v-data-table
    :headers="headers"
    :items="farms"
    :items-per-page="8"
    class="elevation-1"
  >
    <template v-slot:top>
      <EditItem
        v-bind:farms="farms"
        v-bind:editedItem="editedItem"
        v-bind:editedIndex="editedIndex"
        v-bind:defaultItem="defaultItem"
        v-bind:dialog="dialog"
        v-on:editItem="editTableItem($event)"
        v-on:sendData="sendData($event)"
        v-bind:item="item"
        ref="edit"
      />
      <DeleteItem
        v-bind:farms="farms"
        v-bind:editedIndex="editedIndex"
        v-bind:defaultItem="defaultItem"
        v-bind:dialogDelete="dialogDelete"
        
        v-bind:item="item"
        ref="delete"
        v-on:sendData="sendDeleteData($event)"
      />
     
    </template>
    <template v-slot:[`item.edit`]="{ item }">
      <!-- <v-icon small class="mr-2" @click="editItem(item)"> mdi-pencil </v-icon> -->
      <!-- <v-icon small class="mr-2" @click="editItem(item)"> Edit </v-icon> -->
      <v-btn
        class="mx-2"
        fab
        dark
        small
        color="primary"
        @click="submit"
      >
      <v-icon small class="mr-2" @click="editItem(item)">Edit</v-icon>
      
      </v-btn>
    </template>
    <template v-slot:[`item.delete`]="{ item }">
      <!-- <v-icon small @click="deleteItem(item)"> mdi-delete </v-icon> -->
      <!-- <v-icon small @click="deleteItem(item)"> Delete </v-icon> -->
      <v-btn
        class="mx-2"
        fab
        dark
        small
        color="primary"
        @click="submit"
      >
      <v-icon small @click="deleteItem(item)"> Delete </v-icon>
      
      </v-btn>
      
    </template>
  </v-data-table>
</template>
<script>
import DeleteItem from "./DeleteItem.vue";
import EditItem from "./EditItem.vue";
export default {
  components: { EditItem, DeleteItem },
  name: "TableList",
  props: ["headers", "farms", "dialog", "dialogDelete"],
  data() {
    return {
      item: {},
      //dialog: false,
      //dialogDelete: false,
      editedIndex: -1,
      editedItem: {
        farm_id:"",
        farm_name: "",
        city: "",
        lat: "",
        long:"",
        country:"",
        
      }, props:['usrToken'],
      defaultItem: {
        farm_id:"",
        farm_name: "",
        city: "",
        lat: "",
        long:"",
        country:"",
       
      },
    };
  },
  
  methods: {
    sendData(proxyEditedItem, proxyEditedIndex, proxyDialog) {
      this.editedItem = proxyEditedItem;
      //this.dialog=proxyDialog;
      this.editedIndex = this.proxyEditedIndex;
    },
    sendDeleteData(proxyEditedItem, proxyEditedIndex, proxyDeleteDialog) {
      this.editedItem = proxyEditedItem;
      this.dialogDelete = proxyDeleteDialog;
      this.editedIndex = this.proxyEditedIndex;
    },
    editItem(item) {
      this.$refs.edit.editItem(item);
    },
    deleteItem(item) {
       console.log("Delete item",item)
      this.$refs.delete.deleteItem(item);
    },
    
  },
};
</script>
