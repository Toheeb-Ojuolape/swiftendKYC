<template>
  <v-container>
    <v-form>
    <v-card class="pa-3" width="600px" style="margin:4% auto 10% auto;max-width:100%">
    <v-card>
    <v-img src="https://www.google.com/webhp?hl=en&sa=X&ved=0ahUKEwiRtuiNz8vwAhXOgVwKHdSAD7UQPAgI" />
    <v-select outlined
    :items="items"
    chips
    label="Select ID Type"
     />
    </v-card>
    
    
    </v-card>
    
    </v-form>
  </v-container>
</template>

<script>
  export default {
    name: 'HelloWorld',

    data: () => ({
     items:['Bank Verificationn Number (BVN)', 'Corporate Affairs Commission (CAC)', 'National Identity Number (NIN)']
    }),
  }
</script>
