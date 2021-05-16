<template>
  <v-app>
    <v-main style="background:#cee9fe">
      <Form/>
    </v-main>
  </v-app>
</template>

<script>
import Form from './components/Form';

export default {
  name: 'App',

  components: {
    Form,
  },

  data: () => ({
    //
  }),
};
</script>
