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
          <h2>Klip 연동 QR</h2>
          <img :src="image">
        </div>
         <v-btn
            color="primary"
            nuxt
            to="/inspire"
            @click="onWeb()"
          >
            웹프라우저로 연결
          </v-btn>
        <div>
          <h2>request_key</h2>
          <p>{{auth.request_key}}</p>
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
import QRCode from 'qrcode'

export default {
  name: 'InspirePage',
    data() {
    return {
      image: '',
      prepare: '',
      auth: '',
      cardInfo: '',
      webUrl: ''
    }
  },
  methods: {
    fetchPrepare() {
      const url = 'https://a2a-api.klipwallet.com/v2/a2a/result?request_key='+ this.auth.request_key;
       const headers = {
        'Content-Type': 'application/json'
      };
      const response = axios.get(url,{headers});
      this.prepare = response.data;
    },
    fetchAuth() {
      const headers = {
        'Content-Type': 'application/json'
      };
      const data = {"bapp": { "name" : "My BApp" }, "type": "auth" };
      axios.post('https://a2a-api.klipwallet.com/v2/a2a/prepare',data,{headers}).then(res=>{
        this.auth = res.data
        this.webUrl = 'https://klipwallet.com/?target=/a2a?request_key='+ this.auth.request_key;
        this.makeQr(this.webUrl)
        this.fetchPrepare();
        // this.getCard();
      });
    },
    onWeb(){
      window.open(this.webUrl);
    },
    makeQr(urlList){
      // With promises
      QRCode.toDataURL(urlList)
        .then(url => {
          console.log(url)
          this.image = url;
        })
        .catch(err => {
          console.error(err)
        })
    },
    getCard(){
      const url = 'https://a2a-api.klipwallet.com/v2/a2a/cards?sca='+ this.auth.request_key;
      const headers = {
        'Content-Type': 'application/json'
      };
      const response = axios.get(url,{headers});
      this.cardInfo = response.data;
    }
  },
  created() {
    this.fetchAuth();
  },
}
</script>
