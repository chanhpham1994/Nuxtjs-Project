<template>
<div class="c-mainvisual">
  <swiper :options="swiperOption" class="js-slider">
    <swiper-slide v-for="(imgItem, index) in carousels" :key="index" :index="index">
      <img v-bind:src="imgItem.url" />
    </swiper-slide>
  </swiper>
</div>
</template>

<script>
import { mapState } from 'vuex';
import { swiper,swiperSlide } from 'vue-awesome-swiper'
export default {
  name: 'Carousel',
  components: {
    swiper,
    swiperSlide
  },
  // async asyncData ({store}) {
  //   // await store.dispatch('loadImages');
  //   return store.dispatch('GET_IMAGE');
  // },
  props: {
    carousel: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      carousels: this.carousel[0].acf.wp_image,
      swiperOption: {
        autoplay: {
          delay: 5000
        },
        slidesPerView: 1,
        effect: 'fade',
      }
    }
  }
}
</script>
