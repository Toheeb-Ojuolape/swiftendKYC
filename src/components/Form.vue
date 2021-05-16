<template>
  <v-container>
    <v-row>
      <v-col md=3 sm=3 lg=3>
      <div style="width:450px;margin:2% auto 10% auto;max-width:100%">
        <h2 class="mt-6">Customization Options</h2>
        <p>Choose Theme Color:</p>
        <v-color-picker v-model="color"></v-color-picker>
        <p style="margin-top:20px;margin-bottom:-10px">Form Shadow</p>
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
        </div>
      </v-col>
      <v-col>
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
                  src="https://i.ibb.co/XykkMg7/logo-blue.png"
                  height="50px"
                  width="50px"
                  style="text-align:center;margin-bottom:20px"
                />
              </div>
              <smart-camera-web>
          </smart-camera-web>
              <v-select
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
                label="ID No."
                outlined
                :color="color"
                single-line
                style="margin-bottom:-5px"
                dense
              />
              <v-select
                outlined
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
                  <div style="border-radius:4px;padding:3px 10px 3px 10px;border:1px solid" class="v-input__slot">
                    <div  class="v-text-field__slot">
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
              <v-checkbox
                label="I hereby consent to providing my details to Swiftend"
                :color="color"
                class="black--text"
                style="margin-top:10px"
                v-model="checkbox"
              >
              </v-checkbox>

              <v-btn :color="showColor" block class="white--text">
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
              >CLOSE</v-btn
            >
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import '@smile_identity/smart-camera-web'
import VueGoogleAutocomplete from "vue-google-autocomplete";

export default {
  name: "Form",
  components: { VueGoogleAutocomplete },

  data: () => ({
    items: [
      "Bank Verificationn Number (BVN)",
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
    color: "#f58634",
    location: {},
    types: ["hex"],
    type: "hex",
    hex: "#FF00FF",
    hexa: "#FF00FFFF",
    rgba: { r: 255, g: 0, b: 255, a: 1 },
    hsla: { h: 300, s: 1, l: 0.5, a: 1 },
    hsva: { h: 300, s: 1, v: 1, a: 1 },
    slider: 0,
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
  },
};
</script>
