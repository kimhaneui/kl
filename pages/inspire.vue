<template>
  <v-row>
    <v-col class="text-center">
      <img
        src="/v.png"
        alt="Vuetify.js"
        class="mb-5"
      >
      <blockquote class="blockquote">
        <footer>
          <small>
            <p>{{user}}</p>
          </small>
        </footer>
      </blockquote>
    </v-col>
  </v-row>
</template>

<script>
import axios from 'axios';
export default {
  name: 'InspirePage',
    data() {
    return {
      user: '',
    }
  },
  methods: {
    async fetchUser() {
      const response = await axios.post('https://api.klipwallet.com/v2/wallet/image');
      this.user = response.data;
      console.log(this.user,'this.user')
    }
  },
  created() {
    this.fetchUser();
  },
}
</script>
