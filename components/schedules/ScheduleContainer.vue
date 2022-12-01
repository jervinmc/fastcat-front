<template>
  <div>
    <div>
     <VueHtml2pdf
        :show-layout="false"
        :float-layout="true"
        :enable-download="true"
        :preview-modal="true"
        :paginate-elements-by-height="1400"
        filename="myPDF"
        :pdf-quality="2"
        :manual-pagination="false"
        pdf-format="a4"
        pdf-orientation="landscape"
        pdf-content-width="800px"
        ref="html2Pdf"
    >
        <section slot="pdf-content">
          <v-row>
            <v-col align="center">
             <div class="text-h4">
              Ship Ticket
             </div>
            </v-col>
          </v-row>
          <v-card class="pa-5" elevation="2">
            <div class="pa-10">
            <v-row>
              <v-col>
                <div class="text-h5">
                 <b>Arrive</b>
                </div>
                <div>
                    {{$route.query.to}}
                </div>
              </v-col>
              <v-col>
                <div class="text-h5">
                 <b>Departure Date</b>
                </div>
                <div>
                    {{$route.query.from}}
                </div>
              </v-col>
              <v-col>
                <div class="text-h5">
                 <b> Class</b>
                </div>
                <div>
                  {{$route.query.ticket_type}}
                </div>
              </v-col>
            </v-row>
             <v-row>
              <v-col>
                <div class="text-h5">
                 <b>Departure Date</b>
                </div>
                <div>
                    {{$route.query.depart}} {{book.departure_time}}
                </div>
              </v-col>
              <v-col>
                <div class="text-h5">
                 <b>Return Date</b>
                </div>
                <div>
                    {{$route.query.return}} {{book.return_time}}
                </div>
              </v-col>
              <v-col>
                <div class="text-h5">
                 <b> Class</b>
                </div>
                <div>
                  {{$route.query.ticket_type}}
                </div>
              </v-col>
            </v-row>
            <div>
              <v-img src="https://www.incimages.com/uploaded_files/image/1920x1080/*Barcode_32896.jpg" height="100" width="100">
                
              </v-img>
            </div>
          </div>
          </v-card>
        </section>
    </VueHtml2pdf>
   </div>
    <v-stepper v-model="e1">
      <v-stepper-header class="white">
        <v-stepper-step :complete="e1 > 1" step="1">Schedules </v-stepper-step>
        <v-divider></v-divider>
        <v-stepper-step :complete="e1 > 2" step="2">
          Passengers
        </v-stepper-step>
        <v-divider></v-divider>
        <v-stepper-step step="3"> Payment </v-stepper-step>
      </v-stepper-header>
      <v-stepper-items>
        <!-- anchor 1 -->
        <v-stepper-content step="1">
          <v-row class="pa-2">
            <v-col cols="8" class="pa-5">
              <div class="pb-5">
                <!-- anchor departure  -->
                <v-card class="pa-0 rounded-md" elevation="5" color="secondary">
                  <div class="pa-10 white--text">
                    <v-row>
                      <v-col align-self="center" align="center">
                        <div>Departure</div>
                        <div class="text-h5">
                          <b>{{ $route.query.from }}</b>
                        </div>
                      </v-col>
                      <v-col align="center" cols="2">
                        <div>To</div>
                        <v-divider vertical dark></v-divider>
                      </v-col>
                      <v-col align-self="center" align="center">
                        <div>Arival</div>
                        <div class="text-h5">
                          <b>{{ $route.query.to }}</b>
                        </div>
                      </v-col>
                    </v-row>
                  </div>
                </v-card>
              </div>
              <div
                class="pb-5"
                v-for="item in searched_query_set_departure"
                :key="item"
              >
                <v-card class="pa-0 rounded-xl" elevation="5">
                  <div>
                    <v-row no-gutters class="pa-0">
                      <v-col align="center" align-self="center">
                        <div class="pt-5">{{ $route.query.ticket_type }}</div>
                      </v-col>
                      <v-col align="center" align-self="center">
                        <div class="pt-5">
                          <span><i>Price</i></span>
                          <b class="text-h5"> P {{ item.price }}</b>
                        </div>
                      </v-col>
                      <v-col
                        no-gutters
                        @click="selectDeparture(item)"
                        align="center"
                        align-self="start"
                        class="text-h6 pa-0"
                        ><div
                          class="secondary rounded-lg pointer py-2 white--text"
                        >
                          <b>Select</b>
                        </div>
                      </v-col>
                    </v-row>
                  </div>
                  <v-divider class="pb-5"></v-divider>
                  <div>
                    <v-row>
                      <v-col align="center">
                        <div class="text-h6">
                          <b>{{ item.package_name }}</b>
                        </div>
                      </v-col>
                      <v-col align="center" align-self="center">
                        <div>{{ item.timefrom }}</div>
                      </v-col>
                      <v-col align="center" align-self="center">
                        {{ item.timeto }}
                      </v-col>
                    </v-row>
                  </div>
                </v-card>
              </div>
              <!-- anchor return  -->
              <div class="py-5" v-if="$route.query.ticket_type=='Round Trip'">
                <v-card class="pa-0 rounded-md" elevation="5" color="secondary">
                  <div class="pa-10 white--text">
                    <v-row>
                      <v-col align-self="center" align="center">
                        <div>Departure</div>
                        <div class="text-h5">
                          <b>{{ $route.query.to }}</b>
                        </div>
                      </v-col>
                      <v-col align="center" cols="2">
                        <div>To</div>
                        <v-divider vertical dark></v-divider>
                      </v-col>
                      <v-col align-self="center" align="center">
                        <div>Arival</div>
                        <div class="text-h5">
                          <b>{{ $route.query.from }}</b>
                        </div>
                      </v-col>
                    </v-row>
                  </div>
                </v-card>
              </div>
              <div
                class="pb-5"
                v-for="item in searched_query_set_return"
                :key="item"
              >
                <v-card class="pa-0 rounded-xl" elevation="5">
                  <div>
                    <v-row no-gutters class="pa-0">
                      <v-col align="center" align-self="center">
                        <div class="pt-5">{{ book.ticket_type }}</div>
                      </v-col>
                      <v-col align="center" align-self="center">
                        <div class="pt-5">
                          <b class="text-h5"> P {{ item.price }}</b>
                        </div>
                      </v-col>
                      <v-col
                        @click="selectReturn(item)"
                        no-gutters
                        align="center"
                        align-self="start"
                        class="text-h6 pa-0"
                        ><div
                          class="secondary rounded-lg pointer py-2 white--text"
                        >
                          <b>Select</b>
                        </div>
                      </v-col>
                    </v-row>
                  </div>
                  <v-divider class="pb-5"></v-divider>
                  <div>
                    <v-row>
                      <v-col align="center">
                        <div class="text-h6">
                          <b>{{ item.package_name }}</b>
                        </div>
                      </v-col>
                      <v-col align="center" align-self="center">
                        <div>{{ item.timefrom }}</div>
                      </v-col>
                      <v-col align="center" align-self="center">
                        {{ item.timeto }}
                      </v-col>
                    </v-row>
                  </div>
                </v-card>
              </div>
            </v-col>
            <!-- anchor departure form -->
            <v-col cols="4" >
              <v-card class="rounded-xl" elevation="5">
                <div class="pa-5 text-h6 secondary white--text">
                  <b>Departure</b>
                </div>
                <div class="pa-5" v-if="book.departure_price != 0">
                  <div class="text-h6 pb-5"><b>FastCat M19</b></div>
                  <div class="text-h6">
                    {{ $route.query.from }} - {{ $route.query.to }}
                  </div>
                  <div class="pt-5">
                    <v-row>
                      <v-col>
                        <div>Departure Date</div>
                        <div class="text-h6">{{ $route.query.depart }}</div>
                      </v-col>
                      <v-col>
                        <div>Departure Time</div>
                        <div class="text-h6">{{ book.departure_time }}</div>
                      </v-col>
                    </v-row>
                  </div>
                  <div class="pt-5">
                    <v-row>
                      <v-col>
                        <div>Accomodation</div>
                        <div class="text-h6">{{ book.ticket_type }}</div>
                      </v-col>
                    </v-row>
                  </div>
                </div>
                <div v-else class="pa-5">Please select package.</div>
                <v-divider></v-divider>
                <div class="pt-5 pa-5">
                  <v-row>
                    <v-col>
                      <div>Price</div>
                      <div class="text-h6">
                        P
                        <b>{{
                          $FormatPrice(
                            parseInt(book.departure_price) *
                              parseInt(book.passenger)
                          )
                        }}</b>
                      </div>
                    </v-col>
                    <!-- <v-col>
                      <div>Cargo Price</div>
                      <div class="text-h6">P <b>0</b></div>
                    </v-col> -->
                  </v-row>
                </div>
              </v-card>
              <div class="pt-5"></div>
              <!-- anchor return form -->
              <v-card class="rounded-xl" elevation="5" v-if="$route.query.ticket_type=='Round Trip'">
                <div class="pa-5 text-h6 secondary white--text">
                  <b>Return</b>
                </div>
                <div class="pa-5" v-if="book.return_price != 0">
                  <div class="text-h6 pb-5"><b>FastCat M19</b></div>
                  <div class="text-h6">
                    {{ $route.query.to }} - {{ $route.query.from }}
                  </div>
                  <div class="pt-5">
                    <v-row>
                      <v-col>
                        <div>Departure Date</div>
                        <div class="text-h6">{{ $route.query.return }}</div>
                      </v-col>
                      <v-col>
                        <div>Departure Time</div>
                        <div class="text-h6">{{ book.return_time }}</div>
                      </v-col>
                    </v-row>
                  </div>
                  <div class="pt-5">
                    <v-row>
                      <v-col>
                        <div>Accomodation</div>
                        <div class="text-h6">{{ book.ticket_type }}</div>
                      </v-col>
                    </v-row>
                  </div>
                </div>
                <div v-else class="pa-5">Please select package.</div>
                <v-divider></v-divider>
                <div class="pt-5 pa-5">
                  <v-row>
                    <v-col>
                      <div>Price</div>
                      <div class="text-h6">
                        P
                        <b>{{
                          $FormatPrice(
                            parseInt(book.return_price) *
                              parseInt(book.passenger)
                          )
                        }}</b>
                      </div>
                    </v-col>
                    <!-- <v-col>
                      <div>Cargo Price</div>
                      <div class="text-h6">P <b>0</b></div>
                    </v-col> -->
                  </v-row>
                </div>
              </v-card>

              <div align="center" class="pt-5">
                <v-btn color="secondary" @click="e1 = e1 + 1"> Continue </v-btn>
              </div>
            </v-col>
          </v-row>
        </v-stepper-content>
        <!-- anchor 2 -->
        <v-stepper-content step="2">
          <v-row class="pa-2">
            <v-col cols="8" class="pa-5">
              <v-card class="pa-5 rounded-xl" elevation="5">
                <v-form
                  v-model="isValid"
                  ref="form"
                  lazy-validation
                  @submit.prevent="submitHandler"
                >
                  <div align="start" class="py-10">
                    <div align="center" class="py-10 secondary--text text-h4">
                      Please fill up the details
                    </div>
                    <div>
                      <v-row>
                        <v-col>
                          Firstname
                          <v-text-field
                            outlined
                            :rules="standardRules"
                            v-model="book.firstname"
                          ></v-text-field>
                        </v-col>
                        <v-col>
                          Lastname
                          <v-text-field
                            outlined
                            :rules="standardRules"
                            v-model="book.lastname"
                          ></v-text-field>
                        </v-col>
                        <v-col>
                          Gender
                          <v-text-field
                            outlined
                            :rules="standardRules"
                            v-model="book.gender"
                          ></v-text-field>
                        </v-col>
                      </v-row>
                    </div>
                    <div>
                      <v-row>
                        <v-col>
                          Email Address
                          <v-text-field
                            outlined
                            :rules="standardRules"
                            v-model="book.email"
                          ></v-text-field>
                        </v-col>
                      </v-row>
                    </div>
                    <div>
                      <v-row>
                        <v-col>
                          Mobile Number
                          <v-text-field
                            outlined
                            :rules="standardRules"
                            v-model="book.mobile_number"
                          ></v-text-field>
                        </v-col>
                        <v-col>
                          Birthdate
                          <v-text-field
                            outlined
                            :rules="standardRules"
                            v-model="book.birthdate"
                          ></v-text-field>
                        </v-col>
                      </v-row>
                    </div>
                    <div>
                      <v-row>
                        <v-col>
                          Select Province
                          <v-select
                            outlined
                            :items="$provinces()"
                            item-text="name"
                            item-value="name"
                            :rules="standardRules"
                            v-model="book.province"
                          ></v-select>
                        </v-col>
                        <v-col>
                          Select City
                          <v-select
                            outlined
                            :items="$cities(book.province)"
                            :rules="standardRules"
                            item-text="name"
                            item-value="name"
                            v-model="book.city"
                          ></v-select>
                        </v-col>
                        <v-col>
                          Select Barangay
                          <v-text-field
                            outlined
                            :rules="standardRules"
                            v-model="book.barangay"
                          ></v-text-field>
                        </v-col>
                      </v-row>
                    </div>
                  </div>
                </v-form>
              </v-card>
            </v-col>
            <v-col cols="4">
              <v-card class="rounded-xl" elevation="5">
                <div class="pa-5 text-h6 secondary white--text">
                  <b>Departure</b>
                </div>
                <div class="pa-5" v-if="book.departure_price != 0">
                  <div class="text-h6 pb-5"><b>FastCat M19</b></div>
                  <div class="text-h6">
                    {{ $route.query.from }} - {{ $route.query.to }}
                  </div>
                  <div class="pt-5">
                    <v-row>
                      <v-col>
                        <div>Departure Date</div>
                        <div class="text-h6">{{ $route.query.depart }}</div>
                      </v-col>
                      <v-col>
                        <div>Departure Time</div>
                        <div class="text-h6">{{ book.departure_time }}</div>
                      </v-col>
                    </v-row>
                  </div>
                  <div class="pt-5">
                    <v-row>
                      <v-col>
                        <div>Accomodation</div>
                        <div class="text-h6">{{ book.ticket_type }}</div>
                      </v-col>
                    </v-row>
                  </div>
                </div>
                <div v-else class="pa-5">Please select package.</div>
                <v-divider></v-divider>
                <div class="pt-5 pa-5">
                  <v-row>
                    <v-col>
                      <div>Price</div>
                      <div class="text-h6">
                        P
                        <b>{{
                          $FormatPrice(
                            parseInt(book.departure_price) *
                              parseInt(book.passenger)
                          )
                        }}</b>
                      </div>
                    </v-col>
                    <!-- <v-col>
                      <div>Cargo Price</div>
                      <div class="text-h6">P <b>0</b></div>
                    </v-col> -->
                  </v-row>
                </div>
              </v-card>
              <div class="pt-5"></div>
              <!-- anchor return form -->
              <v-card class="rounded-xl" elevation="5" v-if="$route.query.ticket_type=='Round Trip'">
                <div class="pa-5 text-h6 secondary white--text">
                  <b>Return</b>
                </div>
                <div class="pa-5" v-if="book.return_price != 0">
                  <div class="text-h6 pb-5"><b>FastCat M19</b></div>
                  <div class="text-h6">
                    {{ $route.query.to }} - {{ $route.query.from }}
                  </div>
                  <div class="pt-5">
                    <v-row>
                      <v-col>
                        <div>Departure Date</div>
                        <div class="text-h6">{{ $route.query.return }}</div>
                      </v-col>
                      <v-col>
                        <div>Departure Time</div>
                        <div class="text-h6">{{ book.return_time }}</div>
                      </v-col>
                    </v-row>
                  </div>
                  <div class="pt-5">
                    <v-row>
                      <v-col>
                        <div>Accomodation</div>
                        <div class="text-h6">{{ book.ticket_type }}</div>
                      </v-col>
                    </v-row>
                  </div>
                </div>
                <div v-else class="pa-5">Please select package.</div>
                <v-divider></v-divider>
                <div class="pt-5 pa-5">
                  <v-row>
                    <v-col>
                      <div>Price</div>
                      <div class="text-h6">
                        P
                        <b>{{
                          $FormatPrice(
                            parseInt(book.return_price) *
                              parseInt(book.passenger)
                          )
                        }}</b>
                      </div>
                    </v-col>
                    <!-- <v-col>
                      <div>Cargo Price</div>
                      <div class="text-h6">P <b>0</b></div>
                    </v-col> -->
                  </v-row>
                </div>
              </v-card>
              <div class="pt-5"></div>
              <v-card class="rounded-xl" elevation="5" color="secondary">
                <div class="pa-5 secondary white--text" align="center">
                  <b>Departure</b>
                </div>
                <div class="pa-5">
                  <div class="white--text">
                    <v-row>
                      <v-col>
                        <div>Passenger {{ book.passenger }}</div>
                      </v-col>
                      <v-col align="end">
                        <div>
                          P
                          <b>{{
                            $FormatPrice(
                              parseInt(book.departure_price) *
                                parseInt(book.passenger)
                            )
                          }}</b>
                        </div>
                      </v-col>
                    </v-row>
                  </div>
                  <div class="pa-5 secondary white--text" align="center" v-if="$route.query.ticket_type=='Round Trip'">
                    <b>Return</b>
                  </div>
                  <div class="white--text">
                    <v-row>
                      <v-col>
                        <div>Passenger {{ book.passenger }}</div>
                      </v-col>
                      <v-col align="end">
                        <div>
                          P
                          <b>{{
                            $FormatPrice(
                              parseInt(book.return_price) *
                                parseInt(book.passenger)
                            )
                          }}</b>
                        </div>
                      </v-col>
                    </v-row>
                  </div>
                </div>
                <div>
                  <v-divider color="white"></v-divider>
                </div>
                <!-- <div class="pa-5">
                  <div class="white--text">
                    <v-row>
                      <v-col>
                        <div>1 X Passenger</div>
                      </v-col>
                      <v-col align="end">
                        <div>912.00</div>
                      </v-col>
                    </v-row>
                  </div>
                  <div class="white--text">
                    <v-row>
                      <v-col>
                        <div>Insurance Total</div>
                      </v-col>
                      <v-col align="end">
                        <div>20.00</div>
                      </v-col>
                    </v-row>
                  </div>
                  <div class="white--text">
                    <v-row>
                      <v-col>
                        <div>Cargo Total</div>
                      </v-col>
                      <v-col align="end">
                        <div>20.00</div>
                      </v-col>
                    </v-row>
                  </div>
                  <div class="white--text">
                    <v-row>
                      <v-col>
                        <div>Convenience Fee</div>
                      </v-col>
                      <v-col align="end">
                        <div>20.00</div>
                      </v-col>
                    </v-row>
                  </div>
                  <div class="white--text">
                    <v-row>
                      <v-col>
                        <div>Terminal Fee</div>
                      </v-col>
                      <v-col align="end">
                        <div>0</div>
                      </v-col>
                    </v-row>
                  </div>
                </div> -->
                <v-divider color="white"></v-divider>
                <div class="pt-5 pa-5">
                  <v-row>
                    <v-col align="end">
                      <div class="white--text">Total</div>
                      <div class="text-h6 white--text">
                        P
                        <b>{{
                          $FormatPrice(
                            (parseInt(book.return_price) +
                              parseInt(book.departure_price)) *
                              parseInt(book.passenger)
                          )
                        }}</b>
                      </div>
                    </v-col>
                  </v-row>
                </div>
              </v-card>
              <div align="center" class="pt-5">
                <v-btn color="secondary" @click="e1 = e1 + 1"> Continue </v-btn>
              </div>
            </v-col>
          </v-row>
        </v-stepper-content>
        <!-- anchor 3 -->
        <v-stepper-content step="3">
          <v-row class="pa-2">
            <v-col cols="8" class="pa-5">
              <v-card class="pa-5 rounded-xl" elevation="5"> </v-card>
            </v-col>
            <v-col cols="4">
              <div class="pt-5"></div>
              <v-card class="rounded-xl" elevation="5" color="secondary">
                <div class="pa-5 secondary white--text" align="center">
                  <b>Departure</b>
                </div>
                <div class="pa-5">
                  <div class="white--text">
                    <v-row>
                      <v-col>
                        <div>Passenger {{ book.passenger }} (Minor)</div>
                      </v-col>
                      <v-col align="end">
                        <div>
                          {{
                            $FormatPrice(
                              parseInt(book.departure_price) *
                                parseInt(book.passenger)
                            )
                          }}
                        </div>
                      </v-col>
                    </v-row>
                  </div>
                  <div class="pa-5 secondary white--text" align="center" v-if="$route.query.ticket_type=='Round Trip'">
                    <b>Return</b>
                  </div>
                  <div class="white--text">
                    <v-row>
                      <v-col>
                        <div>Passenger {{ book.passenger }} (Minor)</div>
                      </v-col>
                      <v-col align="end">
                        <div>
                          {{
                            $FormatPrice(
                              parseInt(book.return_price) *
                                parseInt(book.passenger)
                            )
                          }}
                        </div>
                      </v-col>
                    </v-row>
                  </div>
                </div>
                <div>
                  <v-divider color="white"></v-divider>
                </div>
                <div class="pa-5">
                  <div class="white--text">
                    <v-row>
                      <v-col>
                        <div>{{ book.passenger }} X Passenger</div>
                      </v-col>
                      <v-col align="end">
                        <div>
                          {{
                            $FormatPrice(
                              (parseInt(book.departure_price) +
                                parseInt(book.return_price)) *
                                parseInt(book.passenger)
                            )
                          }}
                        </div>
                      </v-col>
                    </v-row>
                  </div>
                  <!-- <div class="white--text">
                    <v-row>
                      <v-col>
                        <div>Insurance Total</div>
                      </v-col>
                      <v-col align="end">
                        <div>20.00</div>
                      </v-col>
                    </v-row>
                  </div>
                  <div class="white--text">
                    <v-row>
                      <v-col>
                        <div>Cargo Total</div>
                      </v-col>
                      <v-col align="end">
                        <div>20.00</div>
                      </v-col>
                    </v-row>
                  </div>
                  <div class="white--text">
                    <v-row>
                      <v-col>
                        <div>Convenience Fee</div>
                      </v-col>
                      <v-col align="end">
                        <div>20.00</div>
                      </v-col>
                    </v-row>
                  </div>
                  <div class="white--text">
                    <v-row>
                      <v-col>
                        <div>Terminal Fee</div>
                      </v-col>
                      <v-col align="end">
                        <div>0</div>
                      </v-col>
                    </v-row>
                  </div> -->
                </div>
                <v-divider color="white"></v-divider>
                <div class="pt-5 pa-5">
                  <v-row>
                    <v-col align="end">
                      <div class="white--text">Total</div>
                      <div class="text-h6 white--text">
                        P
                        <b>{{
                          $FormatPrice(
                            (parseInt(book.departure_price) +
                              parseInt(book.return_price)) *
                              parseInt(book.passenger)
                          )
                        }}</b>
                      </div>
                    </v-col>
                  </v-row>
                </div>
              </v-card>
              <div align="center" class="pt-5">
                <v-btn color="secondary" @click="submitBook"> Continue </v-btn>
              </div>
            </v-col>
          </v-row>
        </v-stepper-content>
      </v-stepper-items>
    </v-stepper>
  </div>
</template>

<script>
import validations from "@/utils/validations";
import { mapState, mapActions } from "vuex";
import VueHtml2pdf from 'vue-html2pdf'
export default {
  components:{
    VueHtml2pdf
  },
  computed: {
    ...mapState("departure", ["searched_query_set_departure"]),
    ...mapState("return", ["searched_query_set_return"]),
  },
  created() {
    if (this.$auth.loggedIn) {
      this.book.firstname = this.$auth.user.firstname;
      this.book.lastname = this.$auth.user.lastname;
      this.book.gender = this.$auth.user.gender;
      this.book.email = this.$auth.user.email;
      this.book.mobile_number = this.$auth.user.mobile_number;
      this.book.birthdate = this.$auth.user.birthdate;
      this.book.address = this.$auth.user.address;
      this.book.province = this.$auth.user.province;
      this.book.barangay = this.$auth.user.barangay;
      this.book.city = this.$auth.user.city;
      this.book.user_id = this.$auth.user.id;
    }
    this.book.departure_from = this.$route.query.from;
    this.book.departure_to = this.$route.query.to;
    this.book.date_from = this.$route.query.depart;
    this.book.ticket_type = this.$route.query.ticket_type;
    this.book.date_to = this.$route.query.return;
    this.book.passenger = this.$route.query.passenger;
    this.searchTripDeparture();
  },
  methods: {
     generateReport () {
            this.$refs.html2Pdf.generatePdf()
        },
    async submitBook() {
      try {
        if(this.$route.query.ticket_type='One Way'){
          this.book.date_to = this.$route.query.depart
        }
        await this.$store.dispatch("book/addBook", this.book);
        // alert("Successfully Booked!");
        // location = "/";
      } catch (error) {
        alert("error");
      }
      // this.generateReport()
      const responses1 = this.$axios
        .post(`/buy-paymaya/`, {
          price:(parseInt(this.book.return_price) + parseInt(this.book.departure_price)) *
            parseInt(this.book.passenger),
          product: "Ticket",
        })
        .then((res) => {
          window.open(
            JSON.parse(res.data)["redirectUrl"],
            "_blank" // <- This is what makes it open in a new window.
          );
        });
      
    },
    selectReturn(item) {
      this.book.return_package = item.package_name;
      this.book.return_time = item.timefrom;
      this.book.return_package = item.return_package;
      this.book.return_price = item.price;
    },
    selectDeparture(item) {
      this.departure_package = item.package_name;
      this.book.departure_package = item.package_name;
      this.book.departure_time = item.timefrom;
      this.book.departure_price = item.price;
    },
    async searchTripDeparture() {
      await this.$store.dispatch(
        "departure/searchTripDeparture",
        this.$route.query.from
      );
      await this.$store.dispatch(
        "return/searchTripReturn",
        this.$route.query.to
      );
    },
  },
  data() {
    return {
      isValid: false,
      ...validations,
      e1: 1,
      book: {
        ticket_type: "",
        book_type: "",
        firstname: "",
        lastname: "",
        gender: "",
        email: "",
        mobile_number: "",
        birthdate: "",
        province: "",
        city: "",
        barangay: "",
        departure_time: "",
        departure_from: "",
        departure_to: "",
        departure_package: "",
        return_package: "",
        departure_price: 0,
        return_time: "",
        return_price: 0,
        province: "",
        date_from: "",
        date_to: "",
        city: "",
        barangay: "",
      },
    };
  },
};
</script>

<style>
</style>