<template>
  <div>
    <VueHtml2pdf
      :show-layout="false"
      :float-layout="true"
      :enable-download="true"
      :paginate-elements-by-height="1400"
      filename="myPDF"
      :pdf-quality="2"
      :manual-pagination="false"
      pdf-format="a4"
      pdf-orientation="landscape"
      pdf-content-width="800px"
      ref="html2Pdf"
    >
      <section slot="pdf-content" v-if="selectedItem.ticket_type=='One Way'">
        <v-row>
          <v-col>
            <v-row>
              <v-divider></v-divider>
              <v-col align="center">
                <div class="text-h4">Ship Ticket</div>
              </v-col>
            </v-row>

            <v-card class="pa-5" elevation="2">
              <v-row>
                <v-col>
                  <div class="pa-10">
                    <v-row>
                      <v-col>
                        <div class="text-h5">
                          <b>Arrive</b>
                        </div>
                        <div>
                          {{ selectedItem.departure_to }}
                        </div>
                      </v-col>
                      <v-col>
                        <div class="text-h5">
                          <b>Departure Date</b>
                        </div>
                        <div>
                          {{ selectedItem.date_from }}
                          {{ selectedItem.departure_time }}
                        </div>
                      </v-col>
                      <!-- <v-col>
                <div class="text-h5">
                 <b> Class</b>
                </div>
                <div>
                  {{$route.query.ticket_type}}
                </div>
              </v-col> -->
                    </v-row>
                    <v-row>
                      <v-col>
                        <div class="text-h5">
                          <b>Departure Date</b>
                        </div>
                        <div>
                          {{ $route.query.depart }} {{ book.departure_time }}
                        </div>
                      </v-col>
                      <v-col>
                        <div class="text-h5">
                          <b>Return Date</b>
                        </div>
                        <div>
                          {{ selectedItem.date_to }}
                          {{ selectedItem.return_time }}
                        </div>
                      </v-col>
                      <!-- <v-col>
                <div class="text-h5">
                 <b> Class</b>
                </div>
                <div>
                  {{$route.query.ticket_type}}
                </div>
              </v-col> -->
                    </v-row>
                    <div>
                      <v-img
                        src="https://www.incimages.com/uploaded_files/image/1920x1080/*Barcode_32896.jpg"
                        height="100"
                        width="100"
                      >
                      </v-img>
                    </div>
                  </div>
                </v-col>
              </v-row>
            </v-card>
          </v-col>
          <v-col>
            <img src="/images/logo.jpg" height="200" width="150" />
          </v-col>
        </v-row>
        <div>
             <img src="/barcode.png" height="100" width="500" />
        </div>
        <v-divider></v-divider>
      </section>
      <section slot="pdf-content" v-else>
        <v-row>
          <v-col>
            <v-row>
              <v-divider></v-divider>
              <v-col align="center">
                <div class="text-h4">Ship Ticket</div>
              </v-col>
            </v-row>

            <v-card class="pa-5" elevation="2">
              <v-row>
                <v-col>
                  <div class="pa-10">
                    <v-row>
                      <v-col>
                        <div class="text-h5">
                          <b>Arrive</b>
                        </div>
                        <div>
                          {{ selectedItem.departure_to }}
                        </div>
                      </v-col>
                      <v-col>
                        <div class="text-h5">
                          <b>Departure Date</b>
                        </div>
                        <div>
                          {{ selectedItem.date_from }}
                          {{ selectedItem.departure_time }}
                        </div>
                      </v-col>
                    </v-row>
                    <v-row>
                      <v-col>
                        <div class="text-h5">
                          <b>Departure Date</b>
                        </div>
                        <div>
                          {{ $route.query.depart }} {{ book.departure_time }}
                        </div>
                      </v-col>
                      <v-col>
                        <div class="text-h5">
                          <b>Return Date</b>
                        </div>
                        <div>
                          {{ selectedItem.date_to }}
                          {{ selectedItem.return_time }}
                        </div>
                      </v-col>
                      <!-- <v-col>
                <div class="text-h5">
                 <b> Class</b>
                </div>
                <div>
                  {{$route.query.ticket_type}}
                </div>
              </v-col> -->
                    </v-row>
                    <div>
                      <v-img
                        src="https://www.incimages.com/uploaded_files/image/1920x1080/*Barcode_32896.jpg"
                        height="100"
                        width="100"
                      >
                      </v-img>
                    </div>
                  </div>
                </v-col>
              </v-row>
            </v-card>
          </v-col>
          <v-col>
            <img src="/images/logo.jpg" height="200" width="150" />
          </v-col>
        </v-row>
        <div>
             <img src="/barcode.png" height="100" width="500" />
        </div>
        <v-divider></v-divider>
      </section>
    </VueHtml2pdf>
    <v-row>
      <v-col class="px-10" cols="6">
        <v-sheet height="80vh">
          <div align="center" class="pt-10">
            <span class="text-h6 secondary--text"><b>Profile</b></span>
          </div>
          <v-row class="pt-0">
            <v-col cols="auto">
              <div align="center">
                <v-avatar
                  ><v-img
                    src="https://e7.pngegg.com/pngimages/436/585/png-clipart-computer-icons-user-account-graphics-account-icon-vector-icons-silhouette.png"
                  ></v-img
                ></v-avatar>
              </div>
              <v-btn color="black" plain :to="'/client/profile/edit'"
                ><span class="pointer">Edit Account</span></v-btn
              >
            </v-col>
            <v-col>
              <div>
                Name
                <v-text-field
                  outlined
                  readonly
                  hide-details=""
                  v-model="$auth.user.firstname"
                ></v-text-field>
              </div>
              <div>
                Email
                <v-text-field
                  outlined
                  readonly
                  hide-details=""
                  v-model="$auth.user.email"
                ></v-text-field>
              </div>
              <div>
                Mobile Number
                <v-text-field
                  outlined
                  readonly
                  hide-details=""
                  v-model="$auth.user.mobile_number"
                ></v-text-field>
              </div>
              <div>
                Address
                <v-text-field
                  outlined
                  readonly
                  hide-details=""
                  v-model="$auth.user.address"
                ></v-text-field>
              </div>
              <div>
                <v-row>
                  <v-col>
                    Date of Birth
                    <v-text-field
                      v-model="$auth.user.birthdate"
                      outlined
                      readonly
                      hide-details=""
                    ></v-text-field>
                  </v-col>
                  <v-col>
                    Gender
                    <v-text-field
                      v-model="$auth.user.gender"
                      outlined
                      readonly
                      hide-details=""
                    ></v-text-field>
                  </v-col>
                </v-row>
              </div>
            </v-col>
          </v-row>
        </v-sheet>
      </v-col>
      <v-col cols="6">
        <v-sheet v-if="true">
          <div align="center" class="pt-10">
            <span class="text-h6 secondary--text"><b>Booking History</b></span>
          </div>
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
              <template #[`item.opt`]="{ item }">
                <v-menu offset-y z-index="1">
                  <template v-slot:activator="{ attrs, on }">
                    <v-btn icon v-bind="attrs" v-on="on">
                      <v-icon>mdi-dots-horizontal</v-icon>
                    </v-btn>
                  </template>
                  <v-list dense>
                    <v-list-item
                      v-if="item.status != 'Approved'"
                      @click.stop="updateStatus(item, 'Cancelled')"
                    >
                      <v-list-item-content>
                        <v-list-item-title>Cancel</v-list-item-title>
                      </v-list-item-content>
                    </v-list-item>
                    <!-- <v-list-item @click.stop="deleteItem(item)">
                      <v-list-item-content>
                        <v-list-item-title>Delete</v-list-item-title>
                      </v-list-item-content>
                    </v-list-item> -->
                    <v-list-item
                      v-if="item.status == 'Approved'"
                      @click.stop="generateTicket(item)"
                    >
                      <v-list-item-content>
                        <v-list-item-title>Generate Ticket</v-list-item-title>
                      </v-list-item-content>
                    </v-list-item>
                  </v-list>
                </v-menu>
              </template>
            </v-data-table>
          </div>
        </v-sheet>
        <div v-else align="center">
          <v-img src="/images/bookVector.jpg" height="450" width="650"></v-img>
        </div>
        <div align="center">
          <v-btn color="secondary" @click="book" outlined class="rounded-lg"
            >Book Now</v-btn
          >
        </div>
      </v-col>
    </v-row>
  </div>
</template>

<script>
import { mapState, mapActions } from "vuex";
import VueHtml2pdf from "vue-html2pdf";
export default {
  components: {
    VueHtml2pdf,
  },
  computed: {
    ...mapState("book", ["query_set"]),
  },
  data() {
    return {
      selectedItem: {},
      isLoading: false,
      users: {
        name: "Jervin Macalawa",
        email: "jmacalawapersonal@gmail.com",
        number: "09056949242",
        address: "Julugan 4 tanza, cavite",
        birthdate: "October 04, 1999",
        gender: "Male",
      },
      items: [],
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
  created() {
    this.loadItem();
  },
  methods: {
    generateTicket(item) {
      console.log(item);
      this.selectedItem = item;
      this.$refs.html2Pdf.generatePdf();
    },
    updateStatus(item, status) {},
    async loadItem() {
      try {
        await this.$store.dispatch("book/viewBookUser", {});
      } catch (error) {
        alert(error);
      }
    },
    book() {
      location = "/schedules";
    },
  },
};
</script>

<style>
</style>