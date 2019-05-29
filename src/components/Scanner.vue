<template>
  <div class="scanner">
    <qrcode-stream @detect="onDetect" v-if="scanner == true"></qrcode-stream>
    <pulse-loader v-if="loading == true"></pulse-loader>
    <h2 v-show="successMsg !== ''"> {{ successMsg  }} &#128591; </h2>
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

        var mahasiswa = {
            name: this.name,
            userId: this.userId,
            presenceId: this.rspn.presenceId,
            presenceNumber: this.rspn.presenceNumber
          }
        
        axios({
          method: 'post',
          url: 'https://fathomless-hollows-37188.herokuapp.com/users/presence',
          data: mahasiswa,
          headers: {
            'Content-Type': 'application/json'
          }
        })
        .then(response => {
          this.loading = false
          this.successMsg = `${response.data.data.name} - ${response.data.message}`
          console.log(response) /* eslint-disable-line no-console */
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