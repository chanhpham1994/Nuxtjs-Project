<template>
<section class="publish">
  <div class="l-container">
    <h2 class="c-title1">
      <span class="ja">出版物</span>
      <span class="en">Publish</span>
    </h2>
    <div class="publish__inner">
      <ul class="c-gridpost">
        <li v-for="(item, index) in publish.slice(0, 4)" :key="index" :index="index" class="c-gridpost__item">
          <!-- Publish Post -->
          <pub-post :item="item"/>
        </li>
      </ul>
    </div>
    <div class="l-btn">
      <div class="c-btn c-btn--small">
        <nuxt-link to="publish">出版物一覧を見る</nuxt-link>
      </div>
    </div>
  </div>
</section>
</template>

<script>
import PubPost from '@/components/PubPost'
export default {
  name: 'Publish',
  components: {
    PubPost
  },
  props: {
    publish: {
      type: Array,
      required: true
    }
  },
  data() {
    return {}
  },
}
</script>
