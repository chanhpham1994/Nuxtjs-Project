<template>
<main class="p-publish">
  <div class="c-breadcrumb">
    <div class="l-container">
      <nuxt-link to="/">Home</nuxt-link>
      <span>出版物</span>
    </div>
  </div>
  <div class="c-headpage">
    <h2 class="c-title">出版物</h2>
    <p>ひかり税理士法人では、税務・会計・経営・相続などに関する書籍の執筆を行っています。</p>
  </div>
  <div class="l-container">
    <div class="p-publish__content">
      <ul class="c-gridpost" id="my-table">
        <li id="my-table" v-for="(item, index) in publish" :key="index" :index="index" class="c-gridpost__item">
          <!-- Publish Post -->
          <pub-post id="my-table" :item="item" />
        </li>
      </ul>
    </div>

    <div class="c-pagination">

      <a v-on:click="prev" class="prev"></a>
      <a v-for="(item, index) in Math.ceil(rows/4)" v-on:click="movePagination(index+1)" v-bind:id="'num'+(index+1)" class="c-btn" :class="{ 'current': index == 0 }">
        {{ index + 1 }}
      </a>
      <a v-on:click="next" class="next"></a>

    </div>

  </div>
</main>
</template>

<script>
import axios from 'axios'
import PubPost from '@/components/PubPost'
export default {
  components: {
    PubPost,
  },
  data() {
    return {
      num: 1,
      publish: this.publish,
    }
  },
  async asyncData({
    app
  }) {
    //GET POST
    const allPublishResponse = await app.$axios.$get(
      'https://test.agl-tool.com/wordpress/?rest_route=/acf/v3/posts/&categories=8'
    )
    const publishResponse = await app.$axios.$get(
      'http://test.agl-tool.com/wordpress/?rest_route=/acf/v3/posts/&categories=8/&per_page=4/&offset=1'
    )
    return {
      allPublish: allPublishResponse,
      publish: publishResponse,
    }
  },
  methods: {
    async movePagination(num) {
      //Xoa current cũ
      const tab = document.getElementsByClassName('c-btn')
      for (let i = 0; i < tab.length; i++) {
        tab[i].classList.remove('current')
      }
      // Add background color
      document.getElementById('num'+num).classList.add('current')

      this.num = num;
      this.publish = []
      this.publish = await this.$axios.$get(
        `http://test.agl-tool.com/wordpress/?rest_route=/acf/v3/posts/&categories=8/&per_page=4/&page=${this.num}`
      )
    },
    async prev() {
      if (this.num > 1) {
        this.num = this.num - 1;
        this.publish = []
        this.publish = await this.$axios.$get(
          `http://test.agl-tool.com/wordpress/?rest_route=/acf/v3/posts/&categories=8/&per_page=4/&page=${this.num}`
        )
        return this.num
      }
      this.num = 1;
    },
    async next() {
      if (this.num < (this.allPublish.length / 4)) {
        this.num = this.num + 1;
        this.publish = []
        this.publish = await this.$axios.$get(
          `http://test.agl-tool.com/wordpress/?rest_route=/acf/v3/posts/&categories=8/&per_page=4/&page=${this.num}`
        )
        return this.num
      }
      this.num = Math.ceil(this.allPublish.length / 5);
    },
  },
  //pagination
  computed: {
    rows() {
      return this.allPublish.length
    }
  }
}
</script>
