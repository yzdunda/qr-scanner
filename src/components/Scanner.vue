<template>
  <div>
    <qrcode-stream @decode="onDecode"></qrcode-stream>
    <p>{{ decodedString }}</p>
  </div>
</template>

<script>
import  { QrcodeStream } from 'vue-qrcode-reader'
export default {
  name: 'Scanner',
  data() {
    return {
      decodedString: ''
    }
  },
  components: {
    QrcodeStream,
  },
  methods: {
    onDecode(decoded) {
      this.decodedString = decoded;
    }
  }
}
</script>

<style scoped>
</style>