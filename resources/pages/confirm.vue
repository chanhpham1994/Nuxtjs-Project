<template>
<main class="p-contact">
  <div class="c-breadcrumb">
    <div class="l-container">
      <nuxt-link to="/">Home</nuxt-link>
      <span>お問い合わせ</span>
    </div>
  </div>
  <div class="c-headpage">
    <div class="l-container2">
      <h2 class="c-title">お問い合わせ</h2>
    </div>
  </div>

  <div class="p-contact__content">
    <div class="l-container">
      <p class="notice">以下の内容を送信してもよろしいですか？</p>

      <table>
        <tr>
          <th>お名前<span class="required">必須</span></th>
          <td>
            <p>{{ firstname }}</p>
            <p>{{ lastname }}</p>
          </td>
        </tr>

        <tr>
          <th>所属団体（社名等）</th>
          <td>
            <p>{{ company }}</p>
          </td>
        </tr>

        <tr>
          <th>E-mail<span class="required">必須</span></th>
          <td>
            <p>{{ email }}</p>
          </td>
        </tr>

        <tr>
          <th>E-mail（確認）<span class="required">必須</span></th>
          <td>
            <p>{{ emailconfirm }}</p>
          </td>
        </tr>

        <tr>
          <th>お電話番号</th>
          <td>
            <p>{{ tel }}</p>
          </td>
        </tr>

        <tr>
          <th>ご相談内容<span class="required">必須</span></th>
          <td>
            <p>{{ message }}</p>
          </td>
        </tr>
      </table>

      <div class="l-btn l-btn--2btn">
        <div class="c-btn">
          <a @click="$router.go(-1)" id="back">前の画面に戻る</a>
        </div>
        <div class="c-btn">
          <nuxt-link to="/complete" id="send">送信する</nuxt-link>
        </div>
      </div>
    </div>
  </div>
</main>
</template>


<script>
export default {
  data(){
    return {
      firstname: '',
      lastname: '',
      company: '',
      email: '',
      emailconfirm:'',
      tel: '',
      message: ''
    }
  },
  mounted() {
    this.firstname = this.$route.params.infors.firstname,
    this.lastname = this.$route.params.infors.lastname,
    this.company = this.$route.params.infors.company,
    this.email = this.$route.params.infors.email,
    this.emailconfirm = this.$route.params.infors.emailconfirm,
    this.tel = this.$route.params.infors.tel,
    this.message = this.$route.params.infors.message
  }
}

</script>
