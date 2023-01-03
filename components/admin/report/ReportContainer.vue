<template>
  <div>
     <v-col>
          <v-card class="pa-5" color="#0086ca" dark>
            <v-row>
              <v-col cols="8">
                <div class="text-h6">
                 Php {{query_set.length}}
                </div>
                <div>
                  Total No. Book
                </div>
              </v-col>
              <v-col align="end">
                  <div>
                    <v-icon>
                      mdi-account
                    </v-icon>   
                  </div>
              </v-col>
            </v-row>
          </v-card>
        </v-col>
        <v-col>
          <v-card class="pa-5" color="#39ca74" dark>
            <v-row>
              <v-col cols="8">
                <div class="text-h6">
                 Php {{totalIncome}}
                </div>
                <div>
                  Registered Users
                </div>
              </v-col>
              <v-col align="end">
                  <div>
                    <v-icon>
                      mdi-account
                    </v-icon>
                  </div>
              </v-col>
            </v-row>
          </v-card>
        </v-col>
        <div>
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
       <template #[`item.amount`]="{ item }">
        {{ parseFloat(item.return_price) + parseFloat(item.departure_price)}}
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
          </v-list>
        </v-menu>
      </template>
    </v-data-table>
        </div>
  </div>
</template>

<script>
import validations from "@/utils/validations";
import { mapState, mapActions } from "vuex";
import DialogConfirmation from "../../general/Dialog/DialogConfirmation.vue";
var cloneDeep = require("lodash.clonedeep");
export default {
  computed: {
    ...mapState("book", ["query_set"]),
    totalIncome(){
        var a = 0.0
        for(let x in this.query_set){
            a = a + ( parseFloat( this.query_set[x].departure_price) + parseFloat(this.query_set[x].return_price) )
        }
        return a
    }
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
        // { text: "Departure Package", value: "departure_package" },
        // { text: "Return Package", value: "return_package" },
        // { text: "Departure Price", value: "departure_price" },
        // { text: "Return Price", value: "return_price" },
        { text: "Amount", value: "amount" },
        { text: "Status", value: "status" },
        // { text: "Actions", value: "opt" },
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