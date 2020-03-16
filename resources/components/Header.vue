<template>
<header class="c-header">
  <div class="l-container">
    <h1 class="c-logo">
      <nuxt-link to="/">
        <img src="~assets/img/logo.png" alt="Allgrow Labo" />
      </nuxt-link>
    </h1>
    <nav class="c-gnav">
      <ul>
        <li v-for="(menuItem, index) in menuItems" :id="index" v-on:click="clickNav(index)" class="c-nav bold">
          <nuxt-link :key="index" v-bind:to="menuItem.linkUrl">
            {{ menuItem.title }}
          </nuxt-link>
        </li>
        <li>
          <nuxt-link to="/contact" >お問い合わせ</nuxt-link>
        </li>
      </ul>
    </nav>
  </div>
</header>
</template>

<script>
export default {
  name: 'Header',
  data() {
    return {
      menuItems: [{
          title: 'サービス',
          titleEn: 'SERVICE',
          linkUrl: '/service'
        },
        {
          title: '出版物',
          titleEn: 'PUBLISH',
          linkUrl: '/publish'
        }
      ]
    }
  },
  methods: {
    clickNav(index) {
      //Xóa style css cho tab
      const tablinks = document.getElementsByTagName('li')
      for (let i = 0; i < tablinks.length; i++) {
        tablinks[i].classList.remove('active')
        tablinks[i].style.backgroundColor = ''
      }
      document.getElementById(index).classList.add('active')
    }
  },
}
</script>

<style lang="scss" scoped>
.c-header {
  position: relative;
  z-index: 999;
  padding: 20px 0 0px;
  -webkit-box-shadow: 0px 2px 5px #333;
  box-shadow: 0px 2px 5px #333;
}

.c-logo {
  display: inline-block;
}
</style>
