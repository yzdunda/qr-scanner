<template>
  <qrcode-stream @decode="onDecode"></qrcode-stream>
</template>

<script>
import  { QrcodeStream } from 'vue-qrcode-reader'
export default {
  name: 'Scanner',
  components: {
    QrcodeStream,
  }
}
</script>

<style scoped>
</style>