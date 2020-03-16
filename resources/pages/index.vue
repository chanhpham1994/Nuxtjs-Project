<template>
<div class="bring-container">
  <Carousel :carousel="carousel" />
  <main class="p-home">
    <section class="service">
      <div class="l-container">
        <h2 class="c-title">
          <span>幅広い案件に対応できるひかりのワンストップサービス</span>目的に応じて、最適な方法をご提案できます
        </h2>
        <div class="service__inner">
          <div class="service__item">
            <img src="~assets/img/img_service01.png" alt="" />
          </div>
          <div class="service__item">
            <img src="~assets/img/img_service02.png" alt="" />
          </div>
          <div class="service__item">
            <img src="~assets/img/img_service03.png" alt="" />
          </div>
          <div class="service__item">
            <img src="~assets/img/img_service04.png" alt="" />
          </div>
        </div>
        <div class="l-btn l-btn--2btn">
          <div class="c-btn">
            <nuxt-link to="/service">ひかり税理士法人のサービス一覧を見る</nuxt-link>
          </div>
          <div class="c-btn">
            <nuxt-link to="/service">ひかり税理士法人の成功事例を見る</nuxt-link>
          </div>
        </div>
      </div>
    </section>
    <News :informationPost="informationPost" :inforCategory="inforCategory" />
    <Publish :publish="publish" />
  </main>
</div>
</template>

<script>
import Carousel from '@/components/Carousel'
import News from '@/components/News'
import Publish from '@/components/Publish'
export default {
  components: {
    Carousel,
    News,
    Publish,
  },
  async asyncData({
    app
  }) {

    const carouselResponse = await app.$axios.$get(
      'https://test.agl-tool.com/wordpress/?rest_route=/acf/v3/pages'
    )
    const inforCategoryResponse = await app.$axios.$get(
      'https://test.agl-tool.com/wordpress/?rest_route=/wp/v2/categories/&parent=14'
    )
    const informationPostResponse = await app.$axios.$get(
      `https://test.agl-tool.com/wordpress/?rest_route=/wp/v2/posts/&tags=12`
    )
    const publishResponse = await app.$axios.$get(
      'https://test.agl-tool.com/wordpress/?rest_route=/acf/v3/posts/&categories=8'
    )
    return {
      informationPost: informationPostResponse,
      carousel: carouselResponse,
      inforCategory: inforCategoryResponse,
      publish: publishResponse,
    }
  },
  mounted() {
    const tablinks = document.getElementsByClassName('c-nav')
    for (let i = 0; i < tablinks.length; i++) {
      tablinks[i].classList.remove('active')
      tablinks[i].style.backgroundColor = ''
    }
  },
}
</script>

<style lang="scss"></style>
