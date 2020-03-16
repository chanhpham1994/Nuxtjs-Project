<template>
<section class="news">
  <div class="l-container">
    <h2 class="c-title1">
      <span class="ja">ニュース</span>
      <span class="en">News</span>
    </h2>
    <div class="news__inner">
      <ul class="c-tabs">
        <!-- subete -->
        <li @click="changeTab" id="all" value="0" class="litab bold">
          すべて
        </li>
        <!-- render per categories -->
        <li v-bind:value="item.id" v-bind:id="item.id" v-for="(item, index) in inforCategory" :key="index" class="litab bold" @click="changeTab">
          {{ item.name }}
        </li>
      </ul>
      <div class="c-tabs__content">
        <!-- All Posts - Display 5 Posts-->
        <ul id="all" class="c-listpost active">
          <!-- NewsPost Component -->
          <news-post :arr="arr" :cateName="cateName" :cateNum="cateNum" :category="inforCategory" :colors="colors"></news-post>
        </ul>
      </div>
    </div>
    <div class="l-btn">
      <div class="c-btn c-btn--small">
        <nuxt-link to="/new">
          ニュース一覧を見る
        </nuxt-link>
      </div>
    </div>
  </div>
</section>
</template>

<script>
import NewsPost from '@/components/NewsPost'
export default {
  name: 'News',
  components: {
    NewsPost
  },
  props: {
    // Categories lấy theo cate cha parent=14
    inforCategory: {
      type: Array,
      required: true
    },
    //Bài Post lấy theo tag=12
    informationPost: {
      type: Array,
      required: true
    },
  },

  data() {
    return {
      cateNum: 0,
      cateName: [],
      arr: this.informationPost,
      activeColor: '#1bb7c5',
      colors: [
        {id: 7,color: '#1bb7c5'},
        {id: 6,color: '#ccc'},
        {id: 3,color: '#416ad3'},
        {id: 5,color: '#c4a021'},
        {id: 4,color: '#d6772a'}
      ]
    }
  },
  methods: {
    async changeTab(event) {
      console.log(event)
      this.arr = []

      // const activeColor = event.target.dataset.color;
      this.cateNum = event.target.value;
      console.log('cateNum', this.cateNum)

      let index = this.colors.findIndex(cl => cl.id == this.cateNum);
      if (index !== -1) {
        console.log(this.colors[index].color)
        this.activeColor = this.colors[index].color
      }

      //Xóa style css cho tab
      const tablinks = document.getElementsByClassName('litab')
      for (let i = 0; i < tablinks.length; i++) {
        tablinks[i].classList.remove('active')
        tablinks[i].style.backgroundColor = ''
      }
      //Thêm style css cho tab được nhấn
      document.getElementById(event.target.id).classList.add('active')
      document.getElementById(event.target.id).style.backgroundColor = this.activeColor;
      document.getElementById(event.target.id).style.borderTopColor = this.activeColor;

      if (this.cateNum == 0) {
        // alert('ALlPost')
        this.cateName = []
        this.arr = await this.$axios.$get(
          `https://test.agl-tool.com/wordpress/?rest_route=/wp/v2/posts/&tags=12`
        )
        this.informationPost.map((item, index) => {
          //So sánh phần tử đầu tiên trong mảng categories của post với id của mảng categories
          this.inforCategory.map((cate, index2) => {
            let i = this.inforCategory.findIndex(sp => cate.id == item.categories[0])
            if (i !== -1) {
              this.cateName = [...this.cateName, cate.name];
              // this.cateNum = cate.id;
            }
          })
        })
      } else {
        // alert('catePost')
        this.cateName = '';
        this.arr = await this.$axios.$get(
          `https://test.agl-tool.com/wordpress/?rest_route=/wp/v2/posts/&categories=${this.cateNum}`
        )
        this.cateName = await this.$axios.$get(
          `https://test.agl-tool.com/wordpress/?rest_route=/wp/v2/categories/${this.cateNum}`
        )
      }
    }
  },
  mounted() {
    //Duyệt mảng chứa bài post
    this.informationPost.map((item, index) => {
      //So sánh phần tử đầu tiên trong mảng categories của post với id của mảng categories
        let i = this.inforCategory.findIndex(sp => sp.id == item.categories[0])
        if (i !== -1) {
          console.log(i)
          this.cateName = [...this.cateName, this.inforCategory[i].name]
          console.log(this.cateName)
          // this.cateNum = cate.id;
        }
    })
    document.getElementById("all").classList.add('active')
  }
}
</script>
