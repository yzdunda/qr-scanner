<template>
  <div class="scanner">
    <qrcode-stream @decode="onDecode"></qrcode-stream>
    <pulse-loader v-if="loading == true"></pulse-loader>
    <p> {{ successMsg  }} </p>
    <p>{{ errorMsg }}</p>
  </div>
</template>

<script>
import  { QrcodeStream } from 'vue-qrcode-reader'
import PulseLoader from 'vue-spinner/src/PulseLoader.vue'
import axios from 'axios'

export default {
  name: 'Scanner',
  data() {
    return {
      decodedString: '',
      loading: false,
      name: "Ghiant Masua Khols",
      userId: 400364,
      successMsg: '',
      errorMsg: ''
    }
  },
  components: {
    QrcodeStream,
    PulseLoader
  },
  methods: {
    onDecode(decoded) {
      this.loading = true;
      let mahasiswa = {
        name: this.name,
        userId: this.userId,
        presenceId: decoded.presenceId,
        presencenumber: decoded.presencenumber
      }

      axios({
        method: 'post',
        url: 'https://fathomless-hollows-37188.herokuapp.com/users/presence',
        data: mahasiswa
      })
      .then(response => {
        this.loading = false;
        this.successMsg = response;
      })
      .catch(error => {
        this.errorMsg = error;
      })


    }
  }
}
</script>

<style scoped>

.scanner {
  text-align: center;
}


</style>