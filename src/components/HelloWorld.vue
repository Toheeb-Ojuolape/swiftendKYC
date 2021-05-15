<template>
  <v-container>
      <v-card
        class="pa-7"
        width="500px"
        style="margin:4% auto 10% auto;max-width:100%"
        elevation=24
      >
        <v-card class="pa-7" style="border:solid;border-color:#0f585b">
        <v-form>
          <div style="text-align:center">
          <img
            src="https://i.ibb.co/XykkMg7/logo-blue.png"
            height="50px"
            width="50px"
            style="text-align:center;margin-bottom:20px"
          />
          </div>
          <!-- <smart-camera-web>
          </smart-camera-web> -->
          <v-select
            outlined
            :items="items"
            dense
            color="#0f585b"
            style="margin-bottom:-5px"
            single-line
            width=60%
            label="Select ID Type"
          />
          <v-text-field
          label="ID No."
          outlined 
          color="#0f585b"
          single-line
          style="margin-bottom:-5px"
          dense
          />
           <v-select
            outlined
            :items="address"
            dense
            color="#0f585b"
            style="margin-bottom:-5px"
            single-line
            width=60%
            label="Select Address Type"
          />
           <v-text-field
          label="Address"
          outlined
          color="#0f585b"
          style="margin-bottom:-5px"
          single-line
          dense
          />
          <v-checkbox label="I hereby consent to providing my details to Swiftend" color="#0f585b" class="black--text" style="margin-top:-5px" v-model="checkbox">
          </v-checkbox>

          <v-btn color="#ee8434" block class="white--text"> Submit </v-btn>
  
          
          </v-form>
        </v-card>
      </v-card>
  </v-container>
</template>

<script>
// import '@smile_identity/smart-camera-web'



export default {
  name: "HelloWorld",

  data: () => ({
    items: [
      "Bank Verificationn Number (BVN)",
      "Corporate Affairs Commission (CAC)",
      "National Identity Number (NIN)",
    ],
    address: [
      'Home Address', 'Business Address', 'Billing Address', 'Shipping Address'
    ],
    checkbox:false
  }),
};
</script>
