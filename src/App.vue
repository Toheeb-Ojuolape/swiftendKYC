<template>
  <v-app>
  <v-window v-model="step">
  <v-window-item :value="1">
    <v-main class="somestyle">
    <div style="margin:39% auto 10% auto;text-align:center">
    <p style="padding:0px 20px 0px 20px;font-size:45px;line-height:50px" class="font-weight-black white--text">Online KYC like no other</p>
    <p style="padding:0px 27px 0px 27px" class="white--text"> Verify your identity to access financial services in less than 30 seconds</p>
    <v-btn x-large rounded @click="step++" color="#f66c1f" class="white--text"> Get Verified</v-btn>
    </div>
    </v-main>
    </v-window-item>
    <v-window-item :value="2">
    <v-main :style="`background:${background}`">
      <Form @stepBack="stepBack" :themesList="themesList" @setTheme="setTheme" />
    </v-main>
    </v-window-item>
    </v-window>
  </v-app>
</template>

<script>
import Form from "./components/Form";

export default {
  name: "App",

  components: {
    Form,
  },

  data: () => ({
    step:1,
    themesList: [
      { color: "#f5f6f9" },
      { color: "#ffd396" },
      { color: "#c9f6cd" },
      { color: "#fbbec3" },
      { color: "#cee9fe" }
    ],
    background:'white'
  }),
  methods:{
    setTheme(index){
      this.background = this.themesList[index].color
    },

    stepBack(){
      this.step = 1
    }
  }
};
</script>


<style>
.somestyle {
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



</style>