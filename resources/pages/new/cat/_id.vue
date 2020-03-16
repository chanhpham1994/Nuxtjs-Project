<template>
<main class="p-news">
  <div class="c-breadcrumb">
    <div class="l-container">
      <nuxt-link to="/">Home</nuxt-link>
      <nuxt-link to="/new">ニュース・お知らせ</nuxt-link>
      <span>{{ cateName }}</span>
    </div>
  </div>
  <div class="c-headpage">
    <h2 class="c-title">ニュース・お知らせ</h2>
  </div>
  <div class="p-news__content">
    <div class="l-container">
      <ul class="c-listpost" id="cat_1">
        <li v-for="(item, index) in post" :key="index" :index="index" class="c-listpost__item">
          <div class="c-listpost__info">
            <span class="datepost">{{ formateDate(item.date) }}</span>
            <span class="cat">

              <i class="c-dotcat" style="background-color: #1bb7c5"></i>

              <span>{{ cate_Name }}</span>
            </span>
          </div>
          <h3 class="titlepost">
            <nuxt-link :to="{
               name: 'new-post-id',
               params: { post: item, id: item.id , cateNum: cateNum, category: category}
            }" v-html="item.title.rendered"></nuxt-link>
          </h3>
        </li>
      </ul>

    </div>
  </div>
</main>
</template>

<script>
export default {
  async asyncData({
    app,
    $axios,
    params,
    error
  }) {
    if (!params.post) {
      //GET POST
      // alert('reload page')
      const id = params.id
      const categoryhResponse = await app.$axios.$get(
        'https://test.agl-tool.com/wordpress/?rest_route=/wp/v2/categories'
      )
      const inforPost = await app.$axios.$get(
        `https://test.agl-tool.com/wordpress/?rest_route=/wp/v2/posts/&categories=${id}`
      )
      return {
        post: inforPost,
        cateNum: id,
        category: categoryhResponse,
      }
    } else {
      // alert('params co du lieu')
      console.log(params)
      return {
        post: params.posts,
        cateName: params.cateName,
        cateNum: params.id,
        category: params.category
      }
    }
  },
  mounted() {
    console.log('cateNum', this.cateNum)
    let index1 = this.category.findIndex(item => item.id == this.cateNum);
    console.log('index1', index1)
    if (index1 !== -1) {
      this.cate_Name = this.category[index1].name;
    }
    console.log('cate_Name', this.cate_Name)
  },
  data() {
    return {
      cate_Name: ''
    }
  },
  methods: {
    formateDate(date) {
      const str = date.trim()
      const y = str.substr(0, 4)
      const m = str.substr(5, 2)
      const d = str.substr(8, 2)
      return y + '年' + m + '月' + d + '日'
    },
    async movePagination(num) {
      this.publish = []
      this.publish = await this.$axios.$get(
        `https://test.agl-tool.com/wordpress/?rest_route=/wp/v2/posts/&tags=12/&per_page=5/&page=${num}`
      )
    }
  },

}
</script>
