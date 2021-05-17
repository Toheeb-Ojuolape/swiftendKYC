<template>
  <v-container>
    <v-dialog v-model="dialog">
      <v-card width="400px" class="py-1 px-5 pb-9">
        <div style="width:450px;margin:2% auto 10% auto;max-width:100%">
          <h3 class="mt-6">Customization Options</h3>
          <p>Choose Theme Color:</p>
          <v-color-picker v-model="color"></v-color-picker>
          <p style="margin-top:10px;margin-bottom:-10px">Form Elevation</p>
          <div style="width:276px">
            <v-slider
              v-model="slider"
              thumb-label
              height="50"
              dense
              class="mt-0"
              style="margin-top:-30px"
              @change="sliderChange(slider)"
              max="24"
            ></v-slider>
          </div>
          <p style="margin-top:-10px;margin-bottom:-4px">Background Color</p>
          <div
            v-for="(color, index) in themesList"
            :key="index"
            style="float:left;margin:5px 5px 30px 5px;"
          >
            <v-btn x-small :color="color.color" fab @click="setTheme(index)" />
          </div>
        </div>
        
      </v-card>
    </v-dialog>

    <v-dialog v-model="logo">
      <v-card class="pa-4" style="margin:auto" width="300px">
        <h2>Upload your logo</h2>
        <v-text-field
          v-model="logoLink"
          prepend-inner-icon="mdi-camera"
          label="Paste a link to your logo image here"
        />
        <v-btn @click="logo = false" outlined color="#fa9746" class="ma-3">
          Close</v-btn
        >
      </v-card>
    </v-dialog>
    <v-card
      class="pa-7"
      width="450px"
      style="margin:2% auto 10% auto;max-width:100%"
      :elevation="elevation"
    >
      <v-card
        class="pa-7"
        :style="`border-color:${showColor}`"
        style="border:solid"
      >
        <v-form>
          <div style="text-align:center">
            <img
              :src="logoLink"
              height="50px"
              width="50px"
              style="text-align:center;margin-bottom:20px"
            />
          </div>
          <smart-camera-web> </smart-camera-web>
          <v-radio-group row v-model="IDs" v-if="IDs == 'false'">
            <template v-slot:label>
              <div style="font-size:16px">
                Do you want to verify your identity using your BVN, NIN or CAC?
              </div>
            </template>
            <v-radio label="Yes" value="true"></v-radio>
            <v-radio label="No" value="false"></v-radio>
          </v-radio-group>
          <div v-if="IDs == 'true'">
            <v-select
              v-model="idtype"
              outlined
              :items="items"
              dense
              :color="color"
              style="margin-bottom:-5px"
              single-line
              width="60%"
              label="Select ID Type"
            />
            <v-text-field
              v-model="id"
              label="ID No."
              outlined
              :color="color"
              single-line
              style="margin-bottom:-5px"
              dense
            />
          </div>
          <v-radio-group row v-model="addresses" v-if="addresses == 'false'">
            <template v-slot:label>
              <div style="font-size:16px">
                Do you want to verify your identity with your Address?
              </div>
            </template>
            <v-radio label="Yes" value="true"></v-radio>
            <v-radio label="No" value="false"></v-radio>
          </v-radio-group>
          <div v-if="addresses == 'true'">
            <v-select
              outlined
              v-model="addresstype"
              :items="address"
              dense
              :color="showColor"
              style="margin-bottom:-20px"
              single-line
              width="60%"
              label="Select Address Type"
            />
            <div data-v-7ee825f0="" class="v-input theme--light v-text-field">
              <div :color="showColor" class="v-input__control">
                <div
                  style="border-radius:4px;padding:3px 10px 3px 10px;border:1px solid"
                  class="v-input__slot"
                >
                  <div class="v-text-field__slot">
                    <vue-google-autocomplete
                      single-line
                      dense
                      ref="address"
                      id="map"
                      classname="form-control"
                      placeholder="Address"
                      v-on:placechanged="getAddressData"
                      country="ng"
                      enable-geolocation
                    >
                    </vue-google-autocomplete>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <v-checkbox
            label="I hereby consent to providing my details to Swiftend"
            :color="color"
            class="black--text"
            style="margin-top:10px"
            v-model="checkbox"
          >
          </v-checkbox>

          <v-btn
            @click="postData()"
            :disabled="(IDs == 'false' || !checkbox) && (addressses == 'false' || !checkbox)"
            :color="showColor"
            block
            class="white--text"
          >
            Submit
          </v-btn>
        </v-form>
      </v-card>
      <v-card-actions>
        <v-btn
          color="#0f585b"
          small
          style="float:right"
          class="white--text mb-0 mt-1"
          @click="stepBack()"
          >Close</v-btn
        >
      </v-card-actions>
    </v-card>
        <v-speed-dial
            v-model="dialShare"
            absolute
            right
            bottom
            direction="top"
            :color="color"
            :open-on-hover="hover"
          >
            <template v-slot:activator>
              <v-btn
                fab
                bottom
                x-large
                fixed
                :color="color"
                style="margin-left:-80px"
              >
                <v-icon v-if="dialShare" class="white--text">mdi-close</v-icon>
                <v-icon v-else class="white--text">mdi-cog</v-icon>
              </v-btn>
            </template>
            <v-btn
              dark
              fab
              fixed
              bottom
              :color="showColor"
              large
              target="_blank"
              style="margin-left:-75px;margin-bottom:180px"
              @click="dialog = true"
            >
              <v-icon>mdi-cogs</v-icon>
            </v-btn>

            <v-btn
              dark
              fab
              fixed
              bottom
              color="black"
              large            
              target="_blank"
              style="margin-left:-75px;margin-bottom:100px"
              @click="logo=true"
            >
              <v-icon>mdi-camera</v-icon>
            </v-btn>

           
          </v-speed-dial>
         

  </v-container>
</template>

<script>
import "@smile_identity/smart-camera-web";
import VueGoogleAutocomplete from "vue-google-autocomplete";

export default {
  name: "Form",
  components: { VueGoogleAutocomplete },
  props: {
    themesList: Array,
  },
  data: () => ({
    items: [
      "Bank Verification Number (BVN)",
      "Corporate Affairs Commission (CAC)",
      "National Identity Number (NIN)",
    ],
    address: [
      "Home Address",
      "Business Address",
      "Billing Address",
      "Shipping Address",
    ],
    checkbox: false,
    elevation: 0,
    step: 2,
    color: "#f58634",
    location: {},
    id: "",
    idtype: "",
    addresstype: "",
    payload: {},
    types: ["hex"],
    type: "hex",
    hex: "#FF00FF",
    hexa: "#FF00FFFF",
    rgba: { r: 255, g: 0, b: 255, a: 1 },
    hsla: { h: 300, s: 1, l: 0.5, a: 1 },
    hsva: { h: 300, s: 1, v: 1, a: 1 },
    slider: 0,
    dialog: false,
    IDs: "false",
    addresses: "false",
    logo: true,
    logoLink: "",
    hover:false
  }),

  computed: {
    showColor() {
      if (typeof this.color === "string") return this.color;

      return JSON.stringify(
        Object.keys(this.color).reduce((color, key) => {
          color[key] = Number(this.color[key].toFixed(2));
          return color;
        }, {}),
        null,
        2
      );
    },
   
  },

  methods: {
    getAddressData: function(addressData, placeResultData) {
      this.location = {
        exact: placeResultData.formatted_address,
        vicinity: placeResultData.vicinity,
      };
    },
    sliderChange(slider) {
      this.elevation = slider;
    },
    postData() {
      this.payload = {
        ID: this.idtype,
        IDNumber: this.id,
        addresstype: this.addresstype,
        location: this.location.exact,
      };
      console.log(this.payload);
    },
    setTheme(index) {
      this.$emit("setTheme", index);
    },
    stepBack() {
      this.$emit("stepBack");
    },
  },
};
</script>

<style>
.v-btn {
  text-transform: none;
}
</style>
