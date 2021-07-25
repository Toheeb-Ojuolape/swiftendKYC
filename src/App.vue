<template>
  <v-app>
    <v-main>
      <v-card class="pa-6" width="500px" style="margin:120px auto 120px auto">
        <smart-camera-web> </smart-camera-web>
        <v-form class="mt-5">
          <v-select
            outlined
            v-model="idtype"
            label="Select ID type"
            :items="items"
          >
          </v-select>
          <v-text-field outlined v-model="id" label="Enter your NIN number" />
          <v-row>
            <v-col>
              <v-text-field label="First Name" v-model="firstName" outlined />
            </v-col>
            <v-col>
              <v-text-field label="Last Name" v-model="lastName" outlined />
            </v-col>
          </v-row>
        </v-form>
        <v-btn
          :loading="loading"
          block
          @click="submit()"
          x-large
          color="#1877f2"
          class="white--text"
          style="border-radius:8px"
          >Submit</v-btn
        >
      </v-card>
    </v-main>

    <!-- <v-window v-model="step">
  <v-window-item :value="1">
    <v-main class="somestyle">
    <div style="margin:34% auto 10% auto;text-align:center">
    <p style="padding:0px 20px 0px 20px;font-size:45px;line-height:50px" class="font-weight-black white--text">Online KYC like no other</p>
    <p style="padding:0px 27px 0px 27px" class="white--text"> Verify your identity to access financial services in less than 30 seconds</p>
    <v-btn x-large rounded @click="step++" color="#f66c1f" class="white--text"> Get Verified</v-btn>
    </div>
    </v-main>
    </v-window-item>
    <v-window-item :value="2">
    <v-main :style="`background:${background}`">
      <Form @stepBack="stepBack" :themesList="themesList" @changeBackground="changeBackground" />
    </v-main>
    </v-window-item>
    </v-window> -->
  </v-app>
</template>

<script>
// import Form from "./components/Form";
import "@smile_identity/smart-camera-web";
import axios from "axios";

export default {
  name: "App",

  components: {
    // Form,
  },

  data: () => ({
    id: "",
    idtype: "",
    step: 1,
    count: 1,
    userCount: 1,
    firstName: "",
    lastName: "",
    loading: false,
    items: ["NIN"],
    themesList: [
      { color: "#f5f6f9" },
      { color: "#ffd396" },
      { color: "#c9f6cd" },
      { color: "#fbbec3" },
      { color: "#cee9fe" },
    ],
    background: "white",
  }),
  methods: {
    changeBackground(bgcolor) {
      this.background = bgcolor;
    },

    stepBack() {
      this.step = 1;
    },
    submit() {
      this.loading = true;
      this.count++;
      this.userCount = "user " + this.userCount++;
      let payLoad = {
        partner_id:
          "LS0tLS1CRUdJTiBQVUJMSUMgS0VZLS0tLS0KTUlHZk1BMEdDU3FHU0liM0RRRUJBUVVBQTRHTkFEQ0JpUUtCZ1FDMFd4Z3lQcVRXMnNuTWJvQWM3dC90WEZySgo1eklZVlFtdFVRaHRpdUFraE5wRklNSktCWVFid0srQkx5NmlSSWNiMWlVY1JRNXlEdWpyMVFpTEF4U0lqcGhtCmtpTTl3UzdoVlRhQWd1WWNOY1FZRHRBK2J3eFM2emh3eGlMc3JTK3ZtSnpCOW1JV0NoNjM3WFp6dDN1bUJZdXoKUlBJbzVCblduWDNVbHFuaVlRSURBUUFCCi0tLS0tRU5EIFBVQkxJQyBLRVktLS0tLQo=",
        sec_key: "sjjjkjkjpppo",
        timestamp: Date.now(),
        country: "NG",
        id_type: "NIN",
        id_number: this.id,
        callback_url: "https://api.swiftend.com",
        first_name: this.firstName,
        last_name: this.lastName,
        partner_params: {
          job_id: this.count,
          user_id: this.userCount,
        },
      };

      const headers = {
        "Content-Type": "application/json",
        "Authorization":"LS0tLS1CRUdJTiBQVUJMSUMgS0VZLS0tLS0KTUlHZk1BMEdDU3FHU0liM0RRRUJBUVVBQTRHTkFEQ0JpUUtCZ1FDMFd4Z3lQcVRXMnNuTWJvQWM3dC90WEZySgo1eklZVlFtdFVRaHRpdUFraE5wRklNSktCWVFid0srQkx5NmlSSWNiMWlVY1JRNXlEdWpyMVFpTEF4U0lqcGhtCmtpTTl3UzdoVlRhQWd1WWNOY1FZRHRBK2J3eFM2emh3eGlMc3JTK3ZtSnpCOW1JV0NoNjM3WFp6dDN1bUJZdXoKUlBJbzVCblduWDNVbHFuaVlRSURBUUFCCi0tLS0tRU5EIFBVQkxJQyBLRVktLS0tLQo=",
      };

      axios
        .post("https://api.smileidentity.com/v2/verify", payLoad, {
          headers: headers,
          // axios({
          //   method:"post",
          //   url:"https://api.smileidentity.com/v2/verify",
          //    data:{
          //     partner_id:"LS0tLS1CRUdJTiBQVUJMSUMgS0VZLS0tLS0KTUlHZk1BMEdDU3FHU0liM0RRRUJBUVVBQTRHTkFEQ0JpUUtCZ1FDMFd4Z3lQcVRXMnNuTWJvQWM3dC90WEZySgo1eklZVlFtdFVRaHRpdUFraE5wRklNSktCWVFid0srQkx5NmlSSWNiMWlVY1JRNXlEdWpyMVFpTEF4U0lqcGhtCmtpTTl3UzdoVlRhQWd1WWNOY1FZRHRBK2J3eFM2emh3eGlMc3JTK3ZtSnpCOW1JV0NoNjM3WFp6dDN1bUJZdXoKUlBJbzVCblduWDNVbHFuaVlRSURBUUFCCi0tLS0tRU5EIFBVQkxJQyBLRVktLS0tLQo=",
          //     sec_key:"sjjjkjkjpppo",
          //     timestamp:Date.now(),
          //     country:'NG',
          //     id_type:'NIN',
          //     id_number:this.id,
          //     callback_url:"https://api.swiftend.com",
          //     first_name:this.firstName,
          //     last_name:this.lastName,
          //     partner_params:{
          //       job_id:this.count,
          //       user_id:this.userCount
          //     }
          //   },
          //   headers:{
          //     Authorization:"Bearer LS0tLS1CRUdJTiBQVUJMSUMgS0VZLS0tLS0KTUlHZk1BMEdDU3FHU0liM0RRRUJBUVVBQTRHTkFEQ0JpUUtCZ1FDMFd4Z3lQcVRXMnNuTWJvQWM3dC90WEZySgo1eklZVlFtdFVRaHRpdUFraE5wRklNSktCWVFid0srQkx5NmlSSWNiMWlVY1JRNXlEdWpyMVFpTEF4U0lqcGhtCmtpTTl3UzdoVlRhQWd1WWNOY1FZRHRBK2J3eFM2emh3eGlMc3JTK3ZtSnpCOW1JV0NoNjM3WFp6dDN1bUJZdXoKUlBJbzVCblduWDNVbHFuaVlRSURBUUFCCi0tLS0tRU5EIFBVQkxJQyBLRVktLS0tLQo=",
          //     'Content-Type':'application/json'
          //   }
        })
        .then((response) => {
          console.log(response);
          this.loading = false;
        })
        .catch((error) => {
          console.log(error);
          this.loading = false;
        });
    },
  },
};
</script>

<style>
/*.somestyle {
  background-image: url(https://i.ibb.co/4ZWQqWX/Untitled-design-72.png);
  background-size:cover;
  width:100%;
}

@media only screen and (max-width: 1440px) {
.somestyle {
  background-image: url(https://i.ibb.co/4ZWQqWX/Untitled-design-72.png);
  background-size:cover;
  width:100%;
  height:1000px
}
}

@media only screen and (max-width: 1200px) {
.somestyle {
  background-image: url(https://i.ibb.co/4ZWQqWX/Untitled-design-72.png);
  background-size:cover;
  width:100%;
  height:780px
}
}
*/
</style>
