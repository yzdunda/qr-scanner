<template>
  <div class="scanner">
    <qrcode-stream @detect="onDetect" v-if="scanner == true"></qrcode-stream>
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
      scanner: true,
      loading: false,
      name: "Ghiant Masua Khols",
      userId: 400364,
      rspn: '',
      successMsg: '',
      errorMsg: ''
    }
  },
  components: {
    QrcodeStream,
    PulseLoader
  },
  methods: {
    async onDetect (promise) {

      try {
        let response = await promise
        let responseContent = response.content
        let rspn = JSON.parse(responseContent)
        this.rspn = rspn
      } catch (error) {
        this.errorMsg = error;
      } finally {
        this.loading = true
        this.scanner = false

        let mahasiswa = {
            name: this.name,
            userId: this.userId,
            presenceId: this.rspn.presenceId,
            presencenumber: this.rspn.presencenumber
          }

          axios({
            method: 'post',
            url: 'https://fathomless-hollows-37188.herokuapp.com/users/presence',
            data: mahasiswa
          })
          .then(response => {
            this.scanner = false
            this.loading = false
            this.successMsg = response
          })
          .catch(error => {
            this.loading = false
            this.errorMsg = error
          })
      }

    }
  }
}
</script>

<style scoped>

.scanner {
  text-align: center;
}


</style>