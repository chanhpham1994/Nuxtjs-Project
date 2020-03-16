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
      <h3>メールでのお問い合わせ</h3>
      <p class="notice">下記に必要事項をご記入の上送信下さい。弊所のコンサルタントからご連絡をさせて頂きます。</p>
      <form id="form" >
        <table>
          <tr>
            <th>お名前<span class="required">必須</span></th>
            <td>
              <input type="text" name="firstname" placeholder="姓" v-model="form.firstname" />
              <input type="text" name="lastname" placeholder="名" v-model="form.lastname" />
              <div class="u-warning">{{ errors.name }}</div>
            </td>
          </tr>

          <tr>
            <th>所属団体（社名等）</th>
            <td>
              <input type="text" name="company" v-model="form.company">
            </td>
          </tr>

          <tr>
            <th>E-mail<span class="required">必須</span></th>
            <td>
              <input type="text" name="email" placeholder="例） example@gmail.com" v-model="form.email" required>
              <div class="u-warning">{{ errors.email }}</div>
            </td>
          </tr>

          <tr>
            <th>E-mail（確認）<span class="required">必須</span></th>
            <td>
              <input type="text" name="emailconfirm" placeholder="例） example@gmail.com" v-model="form.emailconfirm" required>
              <div class="u-warning">{{ errors.emailconfirm }}</div>
            </td>
          </tr>

          <tr>
            <th>お電話番号</th>
            <td>
              <input type="tel" name="tel" placeholder="例） 000-1111-2222" v-model="form.tel">
            </td>
          </tr>

          <tr>
            <th>ご相談内容<span class="required">必須</span></th>
            <td>
              <textarea name="message" v-model="form.message" required></textarea>
              <div class="u-warning">{{ errors.message }}</div>
            </td>
          </tr>
        </table>

        <div class="l-btn l-btn--2btn">
          <div class="c-btn">
            <input type="reset" v-on:click="reset" name="" value="リセット">
          </div>

          <div class="c-btn">
            <a v-if="!complete" v-on:click="checkForm"> 入力内容を確認する </a>
            <nuxt-link v-else :to="{
               name: 'confirm',
               params: { infors: form }
            }">入力内容を確認する</nuxt-link>
          </div>

        </div>
      </form>
    </div>
  </div>
</main>
</template>

<script>
export default {
  data() {
    return {
      complete: false,
      errors: {
        name: '',
        email: '',
        emailconfirm: '',
        message: ''
      },
      form: {
        firstname: null,
        lastname: null,
        company: null,
        email: null,
        emailconfirm: null,
        tel: null,
        message: null
      }
    }
  },
  methods: {
    checkForm: function (e) {

      if (!this.form.firstname || !this.form.lastname) {
        this.errors.name = "Vui lòng nhập họ và tên"
      } else {
        this.errors.name = ''
      }

      if (!this.form.email) {
        this.errors.email = "Vui lòng nhập Email"
      } else {
        this.errors.email = ''
      }

      if (!this.form.emailconfirm) {
        this.errors.emailconfirm = "Vui lòng xác nhận Email"
      } else {
        this.errors.emailconfirm = ''
      }

      if (!this.form.message) {
        this.errors.message = "Vui lòng nhập nội dung"
      } else {
        this.errors.message = ''
      }

      // Nếu errors = '' thì complete => true
      for (const i in this.errors) {
        if (this.errors[i] == '') {
          return this.complete = true
        }
        return this.complete = false
      }

      e.preventDefault();
    },
    reset: function () {
      document.getElementById("form").reset();
    }
  }
}
</script>
