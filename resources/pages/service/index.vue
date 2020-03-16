<template>
<main class="p-service">
  <div class="c-breadcrumb">
    <div class="l-container">
      <nuxt-link to="/">Home</nuxt-link>
      <span>ご提供サービス</span>
    </div>
  </div>
  <div class="c-headpage">
    <div class="l-container2">
      <h2 class="c-title">ご提供サービス</h2>
    </div>
  </div>
  <div class="feature_img">
    <img src="~assets/img/img_services01.png" alt="" />
  </div>
  <div class="p-service__content">
    <div class="l-container">
      <p class="notice">ひかり税理士法人がご提供するすべてのサービスを目的別に検索していただけます</p>
      <!-- =======checkArea====== -->
      <div class=" p-service__checkArea">
        <form id="serviceSearch" method="get" action="#">

          <div class="checkArea__form">
            <legend class="servicesList-heading">サービスの対象で選ぶ</legend>
            <div class="checkArea__inner">
              <div class="c-w240">
                <label>
                  <input class="chkbutton" type="checkbox" name="" value="2" checked="">事業者の方</label>
              </div>
              <div class="c-w240">
                <label>
                  <input class="chkbutton" type="checkbox" name="" value="3" checked="">社会福祉法人</label>
              </div>
              <div class="c-w240">
                <label>
                  <input class="chkbutton" type="checkbox" name="" value="4">個人の方</label>
              </div>
              <div class="c-w240">
                <label>
                  <input class="chkbutton" type="checkbox" name="" value="5">医療従事者の方</label>
              </div>
            </div>
          </div>

          <div class="checkArea__form form2">
            <legend class="servicesList-heading">サービスの内容で選ぶ</legend>
            <div class="checkArea__inner">
              <div class="c-w156">
                <label>
                  <input class="chkbutton" type="checkbox" name="" value="6">税務</label>
              </div>
              <div class="c-w156">
                <label>
                  <input class="chkbutton" type="checkbox" name="" value="7">財務</label>
              </div>
              <div class="c-w156">
                <label>
                  <input class="chkbutton" type="checkbox" name="" value="8">相続</label>
              </div>
              <div class="c-w240">
                <label>
                  <input class="chkbutton" type="checkbox" name="" value="9">事業承継</label>
              </div>
              <div class="c-w240">
                <label>
                  <input class="chkbutton" type="checkbox" name="" value="10">ビジネスサポート</label>
              </div>
            </div>
          </div>
        </form>
      </div>

      <p class="p-service__result">23件が該当しました</p>

      <ul class="c-column">
        <li v-for="(item, index) in servicePost" :key="index" :index="index" class="c-column__item">
          <nuxt-link :to="{
             name: 'service-id',
             params: { post: item, id: item.id }
          }">
            <img v-bind:src="item.acf.wp_icon.url" alt="">
            <p>{{ item.acf.wp_title }}</p>
          </nuxt-link>
        </li>
      </ul>

      <div class="endcontent">
        <img src="~assets/img/img_more05.png" alt="">
        <img src="~assets/img/img_more06.png" alt="">
      </div>
    </div>
  </div>
</main>
</template>

<script>
export default {
  async asyncData({
    app
  }) {
    //GET POST
    const serviceResponse = await app.$axios.$get(
      'https://test.agl-tool.com/wordpress/?rest_route=/acf/v3/posts/&categories=10'
    )
    return {
      servicePost: serviceResponse,
    }
  }
}
</script>
