<template>
<main class="p-news">
  <div class="c-breadcrumb">
    <div class="l-container">
      <nuxt-link to="/">Home</nuxt-link>
      <nuxt-link to="/new">ニュース・お知らせ</nuxt-link>
      <span>{{ post.title.rendered }}</span>
    </div>
  </div>

  <div class="p-news__content">
    <div class="l-container">
      <div class="feature_img">
        <img src="~assets/img/img_news.png" alt="">
      </div>

      <div class="c-ttlpostpage">
        <h2>{{ post.title.rendered }}</h2>
        <span>{{ formateDate(post.date) }}</span>
        <span class="c-listpost__cat">
          <i class="c-dotcat" style="background-color: #1bb7c5"></i>
          <span v-for="(item, index) in this.cate_Name">
            {{ cateName == '' ? cateName : item  }} /</span>
        </span>
      </div>

      <div class="single__content" v-html="post.content.rendered"></div>

      <div class="l-btn">
        <div class="c-btn c-btn--small2">
          <nuxt-link to="/new">ニュース一覧を見る</nuxt-link>
        </div>
      </div>
    </div>
  </div>
</main>
</template>

<script>
export default {
  data() {
    return {
      cate_Name: '',
      postsCate: '',
      cateName: ''
    }
  },
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
      const inforPostResponse = await app.$axios.$get(
        `https://test.agl-tool.com/wordpress/?rest_route=/wp/v2/posts/${id}`
      )
      return {
        category: categoryhResponse,
        post: inforPostResponse,
        cateNum: id
      }
    } else {
      // alert('params co du lieu')
      return {
        post: params.post,
        cateName: params.cateName,
        cateNum: params.cateNum,
        category: params.category
      }
    }
  },
  methods: {
    formateDate(date) {
      const str = date.trim()
      const y = str.substr(0, 4)
      const m = str.substr(5, 2)
      const d = str.substr(8, 2)
      return y + '年' + m + '月' + d + '日'
    }
  },
  mounted() {
    //Gán dữ liệu từ params vào data
    this.cateName =  this.$route.params.cateName;

    this.post.categories.map((cate, index) => {
      console.log('item', cate)
      let index1 = this.category.findIndex(item => item.id == cate);
      console.log('index1', index1)
      //hiển thị tất cả categories của bài post
      if (index1 !== -1 && cate !== 9) {
        this.cate_Name = [...this.cate_Name, this.category[index1].name];
      }
    })
  }
}
</script>
