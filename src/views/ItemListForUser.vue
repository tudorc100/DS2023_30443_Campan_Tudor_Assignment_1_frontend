<template>
  <v-card>
    <v-card-title>
      Devices
      <v-spacer></v-spacer>
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
        :items="items"
        :search="search"
    ></v-data-table>
    <ItemDialog
        :opened="dialogVisible"
        :item="selectedItem"
        @refresh="refreshList"
    ></ItemDialog>
  </v-card>
</template>

<script>
import api from "../api";
import ItemDialog from "../components/ItemDialog";

export default {
  name: "ItemList",
  components: { ItemDialog },
  data() {
    return {
      items: [],
      search: "",
      headers: [
        {
          text: "Id",
          align: "start",
          sortable: false,
          value: "id",
        },
        { text: "Description", value: "description" },
        { text: "Address", value: "address" },
        { text: "Consumption", value: "consumption" },
      ],
      dialogVisible: false,
      selectedItem: {},
    };
  },
  methods: {

    async refreshList() {
      this.dialogVisible = false;
      this.selectedItem = {};
      this.items = await api.items.allItemsForUser();
    },
  },
  created() {
    this.refreshList();
  },
};
</script>

<style scoped></style>
