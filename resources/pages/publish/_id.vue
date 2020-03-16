<template>
<main class="p-publish">
  <div class="c-breadcrumb">
    <div class="l-container">
      <nuxt-link to="/">Home</nuxt-link>
      <nuxt-link to="/publish">出版物</nuxt-link>
      <span>{{ post.acf.wp_title }}</span>
    </div>
  </div>

  <div class="l-container">
    <div class="p-publish__single">
      <div class="feature_img">
        <img v-bind:src="post.acf.wp_image.url" alt="">
      </div>
      <div class="p-publish__info">
        <h2>{{ post.acf.wp_title }}</h2>
        <p class="datepost">{{ post.acf.wp_publication_date }} 発行</p>
        <p class="author">
          著者  : {{ post.acf.wp_author }}<br>
          出版社 : {{ post.acf.wp_publisher }}
        </p>
        <p class="price"> {{ post.acf.wp_price }}</p>

        <div class="desc">
          <p> {{ post.acf.wp_desscription }}</p>

          <h4>目次</h4>
          <p v-html="post.acf.wp_contents"></p>
        </div>
      </div>
    </div>
    <div class="l-btn">
      <div class="c-btn c-btn--small2">
        <nuxt-link to="/publish">出版物一覧へ</nuxt-link>
      </div>
    </div>
  </div>
</main>
</template>

<script>
export default {
  asyncData({
    $axios,
    params,
    error
  }) {
    console.log('asyncData post single', params)
    //Get post if params empty
    if (!params.post) {
      const id = params.id
      return $axios
        .get(`https://test.agl-tool.com/wordpress/?rest_route=/acf/v3/posts/${id}`)
        .then((res) => {
          if (res.data.length)
            error({
              statusCode: 404,
              message: 'Post not found!'
            })
          return {
            post: res.data
          }
        })
        .catch(function (ex) {
          console.log('parsing failed', ex)
          error({
            statusCode: 404,
            message: ex
          })
        })
    } else {
      //Get post through params
      return {
        post: params.post
      }
    }
  }
}
</script>
