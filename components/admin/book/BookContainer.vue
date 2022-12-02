<template>
  <v-card elevation="5">
    <dialog-confirmation
      @cancel="cancelConfirmation"
      :isOpen="isConfirmation"
      @confirm="deleteItem"
    />
    <v-dialog v-model="isAddDeparture" width="1000" persistent>
      <v-form
        v-model="isValid"
        ref="form"
        lazy-validation
        @submit.prevent="submitHandler"
      >
        <v-card class="pa-10">
          <div>
            <v-row>
              <v-col>
                Time From
                <v-text-field
                  outlined
                  :rules="standardRules"
                  v-model="departure.timefrom"
                ></v-text-field>
              </v-col>
              <v-col>
                Time To
                <v-text-field
                  outlined
                  :rules="standardRules"
                  v-model="departure.timeto"
                ></v-text-field>
              </v-col>
            </v-row>
          </div>
          <div>
            <v-row>
              <v-col>
                Location
                <v-select
                  :items="trip_selection"
                  outlined
                  v-model="departure.location"
                ></v-select>
              </v-col>
              <v-col>
                Price
                <v-text-field
                  outlined
                  :rules="standardRules"
                  v-model="departure.price"
                ></v-text-field>
              </v-col>
              <v-col cols="12">
                Package Name
                <v-text-field
                  outlined
                  :rules="standardRules"
                  v-model="departure.package_name"
                ></v-text-field>
              </v-col>
            </v-row>
          </div>
          <div class="px-10 pointer" align="center">
            <v-btn width="200" dark depressed color="grey" @click="cancelForm">
              Cancel
            </v-btn>
            <v-btn
              width="200"
              :loading="isLoading"
              dark
              depressed
              color="secondary"
              type="submit"
            >
              Submit
            </v-btn>
          </div>
        </v-card>
      </v-form>
    </v-dialog>
    <v-row>
      <v-col align="start" class="pa-10 text-h5" cols="auto">
        <b>Book Management</b>
      </v-col>
      <v-spacer></v-spacer>
      <v-col align-self="center" align="end" class="pr-10">
        <!-- <v-btn
          class="rnd-btn"
          rounded
          large
          color="secondary"
          depressed
          dark
          width="170"
          @click="openForm"
        >
          <span class="text-none">Add Departure</span>
        </v-btn> -->
      </v-col>
    </v-row>
    <v-data-table
      class="pa-5"
      :headers="headers"
      :items="query_set"
      :loading="isLoading"
    >
      <template v-slot:loading>
        <v-skeleton-loader
          v-for="n in 5"
          :key="n"
          type="list-item-avatar-two-line"
          class="my-2"
        ></v-skeleton-loader>
      </template>
      <template #[`item.opt`]="{ item }">
        <v-menu offset-y z-index="1">
          <template v-slot:activator="{ attrs, on }">
            <v-btn icon v-bind="attrs" v-on="on">
              <v-icon>mdi-dots-horizontal</v-icon>
            </v-btn>
          </template>
          <v-list dense>
            <v-list-item @click.stop="updateStatus(item, 'Approved')">
              <v-list-item-content>
                <v-list-item-title>Approve</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
            <v-list-item @click.stop="updateStatus(item, 'Disapproved')">
              <v-list-item-content>
                <v-list-item-title>Disapprove</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
            <!-- <v-list-item @click.stop="selectItemToDelete(item.id)">
              <v-list-item-content>
                <v-list-item-title>Delete</v-list-item-title>
              </v-list-item-content>
            </v-list-item> -->
          </v-list>
        </v-menu>
      </template>
    </v-data-table>
  </v-card>
</template>

<script>
import validations from "@/utils/validations";
import { mapState, mapActions } from "vuex";
import DialogConfirmation from "../../general/Dialog/DialogConfirmation.vue";
var cloneDeep = require("lodash.clonedeep");
export default {
  computed: {
    ...mapState("book", ["query_set"]),
  },
  components: {
    DialogConfirmation,
  },
  created() {
    this.isLoading = true;
    this.loadItem();
    this.isLoading = false;
  },
  data() {
    return {
      ...validations,
      isValid: false,
      isConfirmation: false,
      isAddDeparture: false,
      buttonLoad: false,
      isAddForm: false,
      isLoading: false,
      isDelete: false,
      selectedItem: {},
      isLoaded: false,
      selectedId: "",
      departure: {
        timefrom: "",
        timeto: "",
        location: "",
        package_name: "",
      },
      trip_selection: [
        "Banago",
        "Batangas",
        "Bredco",
        "Bulalacao",
        "Calapan",
        "Caluya",
        "Caticlan",
        "Cebu",
        "Dapdap",
        "Dapitan",
        "Dumaguette",
        "Dumangas",
        "ilo-ilo",
        "Liloan",
        "Lipata",
      ],
      headers: [
        { text: "ID", value: "id" },
        { text: "Firstname", value: "firstname" },
        { text: "Lastname", value: "lastname" },
        { text: "Departure Package", value: "departure_package" },
        { text: "Return Package", value: "return_package" },
        { text: "Departure Price", value: "departure_price" },
        { text: "Return Price", value: "return_price" },
        { text: "Status", value: "status" },
        { text: "Actions", value: "opt" },
        ,
      ],
    };
  },
  methods: {
    async updateStatus(item, status) {
      try {
        var id = cloneDeep(item.id);
        var stat = cloneDeep(status);
        await this.$store.dispatch("book/updateBookStatus", {
          id: item.id,
          status: stat,
        });
        this.loadItem();
        alert("Successfully Updated!");
      } catch (error) {
        alert(error);
      }
    },
    cancelForm() {
      this.$refs.form.reset();
      this.isAddDeparture = false;
    },
    selectItemToDelete(id) {
      this.selectedId = id;
      this.isConfirmation = true;
    },
    cancelConfirmation() {
      this.isConfirmation = false;
    },
    async deleteItem() {
      try {
        await this.$store.dispatch(
          "departure/deleteDeparture",
          this.selectedId
        );
        this.loadItem();
        this.isConfirmation = false;
        alert("Successfully Deleted");
      } catch (error) {
        alert(error);
        this.isLoading = false;
      }
    },
    async loadItem() {
      try {
        await this.$store.dispatch("book/viewBook", {});
      } catch (error) {
        alert(error);
      }
    },
    cancel() {},
    async openForm() {
      this.isAddForm = true;
      this.isAddDeparture = true;
    },
    async editForm(item) {
      this.departure = cloneDeep(item);
      this.isAddForm = false;
      this.isAddDeparture = true;
    },

    async submitHandler() {
      this.isLoading = true;
      var isValidated = false;
      isValidated = this.$refs.form.validate();
      if (!isValidated) return;
      try {
        if (this.isAddForm)
          await this.$store.dispatch("departure/addDeparture", this.departure);
        else
          await this.$store.dispatch("departure/editDeparture", this.departure);
        this.$refs.form.reset();
        this.loadItem();
        this.isAddDeparture = false;
        this.isLoading = false;
        alert("Successfully Confirmed!");
      } catch (error) {
        alert(error);
        this.isLoading = false;
        this.$refs.form.reset();
      }
    },
  },
};
</script>

<style>
</style>