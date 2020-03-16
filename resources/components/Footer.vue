<template>
<footer class="c-footer">
  <div class="c-footer__logo">
    <div class="l-container">
      <nuxt-link to="/">
        <img src="~assets/img/logo.png" alt="" />
      </nuxt-link>
    </div>
  </div>
  <div class="c-footer__main">
    <div class="l-container">
      <div class="c-footer__link">
        <h3>
          <nuxt-link to="/new">ニュース</nuxt-link>
        </h3>
        <ul class="c-boxlink">

          <li v-for="(item, index) in inforCategory">
            <nuxt-link :to="{
                name: 'new-cat-id',
                params: {id: item.id}
            }"> {{ item.name }} </nuxt-link>
          </li>

        </ul>
      </div>
      <div class="c-footer__link">
        <h3>成功事例</h3>
        <ul class="c-boxlink">
          <li>法人のお客様</li>
          <li>個人のお客様</li>
        </ul>
      </div>
      <div class="c-footer__link">
        <ul class="c-boxlink">
          <li>スタッフ</li>
          <li>採用情報</li>
          <li>プライバシーポリシー</li>
          <li>サイトマップ</li>
        </ul>
      </div>
    </div>
  </div>
</footer>
</template>

<script>
import {
  mapState
} from 'vuex'
export default {
  name: 'Footer',
  data() {
    return {
      inforCategory: []
    }
  },
  mounted() {
    fetch('https://test.agl-tool.com/wordpress/?rest_route=/wp/v2/categories/&parent=14').then(response =>
      response.json().then(data => ({
        data: data,
        status: response.status
      })).then(res => {
        this.inforCategory = res.data
        console.log(res.status, res.data)
    }));
  }
}
</script>
