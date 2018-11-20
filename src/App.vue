<template>
  <div id="app">
    <star-rating
      :increment="increment"
      :rating="rating"
      :max-rating="maxRating"
      :readOnly="readOnly"
      :show-rating="showRating"
      :rtl="rtl"
      :round-star-rating="roundStarRating"
      :star-size="starSize"
      :inactive-color="inactiveColor"
      :active-color="activeColor"
      :border-color="borderColor"
      :border-width="borderWidth"
      :padding="padding"
      :rounded-corners="roundedCorners"
      :inline="inline"
      :glow="glow"
      :glow-color="glowColor"
      :text-class="textClass"
      @rating-selected="onRatingChanged" />
  </div>
</template>

<script>
import LCC from 'lightning-container'
import StarRating from 'vue-star-rating'

export default {
  name: 'app',
  data() {
    return {
      increment: 1,
      rating: 0,
      maxRating: 5,
      readOnly: true,
      showRating: false,
      rtl: false,
      roundStarRating: true,

      starSize: 50,
      inactiveColor: '#d8d8d8',
      activeColor: '#ffd055',
      borderColor: '#999',
      borderWidth: 0,
      padding: 0,
      roundedCorners: false,
      inline: false,
      glow: 0,
      glowColor: '#000',
      textClass: ''
    }
  },
  mounted() {
    this.init()
  },

  methods: {
    onRatingChanged(event) {
      this.rating = event
      this.fireRating()
    },

    fireRating() {
      const payload = {
        channel: 'rating',
        rating: this.rating
      }
      LCC.sendMessage(payload)
    },

    readUrlParams(url) {
      const params = url.substring(url.indexOf('?') + 1).split('&')
        .map(pairStr => {
          const keyValue = pairStr.split('=')

          const pair = {
            key: keyValue[0],
            value: keyValue[1]
          }

          return this.getTypedPair(pair)
        })
        .filter(param => {
          return param.key !== 'sfdcIFrameOrigin' && param.key !== '_CONFIRMATIONTOKEN'
        });

      return params
    },

    getTypedPair(pair) {
      switch(pair.key) {
        case 'rating':
          pair.value = Number(pair.value)
          break;
        case 'maxRating':
          pair.value = Number(pair.value)
          break;
        case 'increment':
          pair.value = Number(pair.value)
          break;
        case 'activeColor':
          pair.value = '#' + pair.value
          break;
        case 'readOnly':
          pair.value = pair.value === 'true'
          break;
        case 'roundedCorners':
          pair.value = pair.value === 'true'
          break;
      }

      return pair
    },

    init() {
      const params = this.readUrlParams(window.location.href)
      params.forEach(param => {
        this[param.key] = param.value
      })
    }
  },
  components: {
    StarRating
  }
}
</script>
