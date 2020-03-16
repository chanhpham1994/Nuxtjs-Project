<template>
<main class="p-service">
  <div class="c-breadcrumb">
    <div class="l-container">
      <nuxt-link to="/">Home</nuxt-link>
      <nuxt-link to="/service">ご提供サービス</nuxt-link>
      <span>{{ post.acf.wp_title }}</span>
    </div>
  </div>
  <div class="c-headpage">
    <div class="l-container2">
      <h2 class="c-title">法人税務顧問</h2>
    </div>
    <p v-html="post.acf.wp_description"></p>
  </div>

  <div class="feature_img">
    <img src="~assets/img/hero_partner.jpg" alt="">
  </div>

  <!-- wp_target -->
  <div class="p-service__consultation">
    <dl class="l-container2">
      <dt>このような方はご相談ください</dt>
      <dd v-for="(item, index) in post.acf.wp_target" class="c-checkMark">
        {{ item.wp_target }}
      </dd>
    </dl>
  </div>

  <!-- wp_advantage -->
  <div class="p-service__merit">
    <div class="l-container2">
      <h3 class="p-service__title">ひかり税理士法人を選ぶメリット</h3>
      <dl v-for="(item, index) in post.acf.wp_advantage">
        <dt class="c-checkMark">{{ item["wp_advantage-title"] }} </dt>
        <dd>{{ item["wp_advantage-description"] }}</dd>
      </dl>
    </div>
  </div>

  <!-- wp_advantage -->
  <div class="p-service__flow">
    <div class="l-container2">
      <h3 class="p-service__title">サービスの流れ</h3>

      <table v-for="(item, index) in post.acf.wp_steps">
        <tbody>
          <tr>
            <th>STEP{{index+1}}</th>
            <td>
              <h4 class="flow-title">{{ item["wp_steps-title"] }}</h4>

              <service :subTitle="item['wp_steps-subtitle']">

              </service>

            </td>
          </tr>
        </tbody>
      </table>

    </div>
  </div>

  <div class="p-service__division">
    <div class="l-container">
      <h3 class="p-service__subtitle">関連サービス</h3>

      <ul class="division-list c-flex">
        <li class="small-12 medium-4">
          <nuxt-link to="/service">
            <p class="img"><img src="~assets/img/1.jpg"></p>
            <p class="text"><span class="arrow">経理改善</span></p>
          </nuxt-link>
        </li>

        <li class="small-12 medium-4">
          <nuxt-link to="/service">
            <p class="img"><img src="~assets/img/1.jpg" alt="1.jpg"></p>
            <p class="text"><span class="arrow">会計顧問</span></p>
          </nuxt-link>
        </li>
      </ul>

    </div>

    <div class="l-btn">
      <div class="c-btn c-btn--small">
        <nuxt-link to="/service">ご提供サービス一覧へ</nuxt-link>
      </div>
    </div>
  </div>

</main>
</template>

<script>
import Service from '@/components/basic/Service'
export default {
  components: {
    Service
  },
  asyncData({
    $axios,
    params,
    error
  }) {
    console.log('asyncData post single', params)

    if (!params.post) {
      // Get post if params empty
      // alert('ko co du lieu')
      const id = params.id
      return $axios
        .get(`https://test.agl-tool.com/wordpress/?rest_route=/acf/v3/posts/${id}`)
        .then((res) => {
          if (res.data.length)
            error({
              statusCode: 404,
              message: 'Post not found!'
            })
          console.log(res.data.acf.wp_advantage)
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
      // Get post through params
      // alert('co du lieu')
      return {
        post: params.post
      }
    }
  }
}
</script>
