<template>
  <v-row>
    <v-col class="text-center">
      <img
        src="/v.png"
        alt="Vuetify.js"
        class="mb-5"
      >
      <blockquote class="blockquote">
        <div>
          <h2>카드 관리(Upload Image)</h2>
          <p>{{image}}</p>
        </div>
        <div>
          <h2>Specification</h2>
          <p>{{prepare}}</p>
        </div>
        <div>
          <h2>Sign In</h2>
          <p>{{auth}}</p>
        </div>
        <footer>
          <small>
            <!-- <p>{{image}}</p> -->
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
      image: '',
      prepare: '',
    }
  },
  methods: {
    fetchUser() {
      const response = axios.post('https://api.klipwallet.com/v2/wallet/image');
      this.image = response.data;
    },
    fetchPrepare() {
      const response = axios.post('https://api.klipwallet.com/v2/a2a/prepare');
      this.prepare = response.data;
    },
    fetchAuth() {
      const response = axios.post('https://api.klipwallet.com/v2/partner/auth');
      this.auth = response.data;
      const url = 'https://api.klipwallet.com/v2/partner/auth';
        const option = {
            d: {"email":"ray.kim@groundx.xyz", "password":"C01069C9ABB6EA7DA49AE418A24BBEF3AD67170DDCD20AC7C76084A5A85E4057"},
            headers: {
                'Content-Type': 'application/json'
            }
        }
        const res = fetch(url,option);
        console.log(res,'res')
    },

  },
  created() {
    this.fetchUser();
    this.fetchPrepare();
    this.fetchAuth();
  },
}
</script>
