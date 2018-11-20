<template>
  <div>
    <star-rating />
  </div>
</template>

<script>
import LCC from 'lightning-container'
import StarRating from 'vue-star-rating'

export default {
  mounted() {
    LCC.sendMessage('starRating mounted')
    LCC.addMessageHandler(message => {
      console.log('message received in star rating')
    })
  },
  components: {
    StarRating
  }
}
</script>
