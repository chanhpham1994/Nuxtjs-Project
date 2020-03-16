<!-- Publish Post -->
<template>
  <div>
    <div class="c-gridpost__thumb">
      <img v-bind:src="item.acf.wp_image.url" alt="">
    </div>
    <div class="c-gridpost__info">
      <p class="datepost bold">{{ item.acf.wp_publication_date }}</p>
      <h3>{{ item.acf.wp_title }}</h3>
      <p class="price">{{ item.acf.wp_price }}</p>
      <nuxt-link :to="{
        name: 'publish-id', // đường dẫn tên của file và file.vue
        params : { post: item, id: item.id}
        }" class="c-btnview">
        詳しく見る
      </nuxt-link>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Pubpost',
  props: {
    item: {
      required: true
    }
  },
  data() {
    return {}
  }
}
</script>
