<template>
<main class="p-news">
  <div class="c-breadcrumb">
    <div class="l-container">
      <nuxt-link to="/">Home</nuxt-link>
      <span>ニュース・お知らせ</span>
    </div>
  </div>
  <div class="c-headpage">
    <h2 class="c-title">ニュース・お知らせ</h2>
  </div>
  <div class="p-news__content">
    <div class="l-container">

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

          <!-- Pagination  -->
          <div class="c-pagination">
            <a v-on:click="prev" class="prev"></a>
              <a v-for="(item, index) in Math.ceil(postLength/5)" v-on:click="movePagination(index+1,cateNum)" v-bind:id="'num'+(index+1)" class="c-btn" :class="{ 'current': index == 0 }">
                {{ index + 1 }}
              </a>
            <a v-on:click="next" class="next"></a>
          </div>
        </ul>
      </div>
    </div>
  </div>
</main>
</template>

<script>
import NewsPost from '@/components/NewsPost'
export default {
  name: 'News',
  components: {
    NewsPost
  },
  async asyncData({
    app
  }) {
    //Lấy tất cả các bài Post
    const allPostResponse = await app.$axios.$get(
      'https://test.agl-tool.com/wordpress/?rest_route=/wp/v2/posts/&tags=12'
    )
    //Lấy tất cả các categories
    const inforCategoryResponse = await app.$axios.$get(
      'https://test.agl-tool.com/wordpress/?rest_route=/wp/v2/categories/&parent=14'
    )
    //Lấy bài post mỗi trang 5 bài
    const informationPostResponse = await app.$axios.$get(
      `https://test.agl-tool.com/wordpress/?rest_route=/wp/v2/posts/&per_page=5&page=1&tags=12`
    )
    return {
      allPost: allPostResponse,
      informationPost: informationPostResponse,
      inforCategory: inforCategoryResponse
    }
  },
  data() {
    return {
      num: 1,
      cateNum: 0,
      cateName: [],
      arr: [],
      postLength: '',
      activeColor: '#1bb7c5',
      colors: [
        {id: 7,color: '#1bb7c5'},
        {id: 6,color: '#ccc'},
        {id: 3,color: '#416ad3'},
        {id: 5,color: '#c4a021'},
        {id: 4,color: '#d6772a'}
      ],
    }
  },
  methods: {
    deleteBgPagination() {
      const tab = document.getElementsByClassName('c-btn')
      for (let i = 0; i < tab.length; i++) {
        tab[i].classList.remove('current')
      }
    },

    async movePagination(num, cateNum) {
      //Xoa current cũ
      this.deleteBgPagination()

      // Add background color
      document.getElementById('num'+num).classList.add('current')

      this.arr = []
      console.log(num)
      console.log(cateNum)
      if (cateNum == 0) {
        // alert(num)
        return this.arr = await this.$axios.$get(
          `https://test.agl-tool.com/wordpress/?rest_route=/wp/v2/posts/&per_page=5&page=${num}&tags=12`
        )
      }
      return this.arr = await this.$axios.$get(
        `https://test.agl-tool.com/wordpress/?rest_route=/wp/v2/posts/&per_page=5&page=${num}&categories=${cateNum}`
      )
    },
    async prev() {
      //Xoa current cũ
      this.deleteBgPagination()

      if (this.num > 1) {
        this.num = this.num - 1;
        this.arr = []
        if (this.cateNum == 0) {
          document.getElementById('num'+this.num).classList.add('current')
          return this.arr = await this.$axios.$get(
            `https://test.agl-tool.com/wordpress/?rest_route=/wp/v2/posts/&per_page=5&page=${this.num}&tags=12`
          )
        }
        this.arr = await this.$axios.$get(
          `https://test.agl-tool.com/wordpress/?rest_route=/wp/v2/posts/&per_page=5&page=${this.num}&categories=${this.cateNum}`
        )
        console.log('this.num',this.num)
        document.getElementById('num'+this.num).classList.add('current')
        return this.num;
      }
      this.num = 1;
    },
    async next() {
      //Xoa current cũ
      this.deleteBgPagination()

      if (this.num < (this.postLength / 5)) {
        this.num = this.num + 1;
        this.arr = []
        if (this.cateNum == 0) {
          document.getElementById('num'+this.num).classList.add('current')
          return this.arr = await this.$axios.$get(
            `https://test.agl-tool.com/wordpress/?rest_route=/wp/v2/posts/&per_page=5&page=${this.num}&tags=12`
          )
        }
        this.arr = await this.$axios.$get(
          `https://test.agl-tool.com/wordpress/?rest_route=/wp/v2/posts/&per_page=5&page=${this.num}&categories=${this.cateNum}`
        )
        console.log(this.num)
        document.getElementById('num'+this.num).classList.add('current')
        return this.num;
      }
      this.num = Math.ceil(this.informationPost.length / 5);
    },
    formateDate(date) {
      const str = date.trim()
      const y = str.substr(0, 4)
      const m = str.substr(5, 2)
      const d = str.substr(8, 2)
      return y + '年' + m + '月' + d + '日'
    },
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
        this.postLength = this.allPost.length
        this.cateName = []
        this.arr = await this.$axios.$get(
          `https://test.agl-tool.com/wordpress/?rest_route=/wp/v2/posts/&per_page=5&page=1&tags=12`
        )
        this.allPost.map((item, index) => {
          //So sánh phần tử đầu tiên trong mảng categories của post với id của mảng categories
          this.inforCategory.map((cate, index2) => {
            let i = this.inforCategory.findIndex(sp => cate.id == item.categories[0])
            if (i !== -1) {
              this.cateName = [...this.cateName, cate.name];
              console.log(this.cateName)
              // this.cateNum = cate.id;
            }
          })
        })
      } else {
        // alert('catePost')
        //Thay đôi độ dài của page => render số page trong pagination
        const postPerCate = await this.$axios.$get(
          `https://test.agl-tool.com/wordpress/?rest_route=/wp/v2/posts/&categories=${this.cateNum}`
        )
        this.postLength = postPerCate.length

        this.cateName = '';
        this.arr = await this.$axios.$get(
          `https://test.agl-tool.com/wordpress/?rest_route=/wp/v2/posts/&categories=${this.cateNum}&per_page=5&page=1`
        )
        this.cateName = await this.$axios.$get(
          `https://test.agl-tool.com/wordpress/?rest_route=/wp/v2/categories/${this.cateNum}`
        )
      }
    }
  },
  mounted() {
    //Độ dài của postLength
    this.postLength = this.allPost.length
    // async => gán giá trị cho biến ARR đã khái báo ở data
    this.arr = this.informationPost
    //Duyệt mảng chứa bài post
    this.informationPost.map((item, index) => {
      //So sánh phần tử đầu tiên trong mảng categories của post với id của mảng categories
        let i = this.inforCategory.findIndex(sp => sp.id == item.categories[0])
        if (i !== -1) {
          console.log(i)
          this.cateName = [...this.cateName, this.inforCategory[i].name]
          // this.cateNum = cate.id;
        }
    })
    document.getElementById('all').classList.add('active')
  },
}
</script>
