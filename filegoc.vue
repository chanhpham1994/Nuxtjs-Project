<template>
<section class="news">
  <div class="l-container">
    <h2 class="c-title1">
      <span class="ja">ニュース</span>
      <span class="en">News</span>
    </h2>
    <div class="news__inner">
      <ul class="c-tabs">
        <li @click="changeTab" value="9" id="all" data-color="#0078d2" class="litab active">
          すべて
        </li>
        <li @click="changeTab" value="7" id="cat_1" data-color="#1bb7c5" class="litab">
          お知らせ
        </li>
        <li @click="changeTab" value="4" id="cat_2" data-color="#d6772a" class="litab">
          税の最新情報
        </li>
        <li @click="changeTab" value="5" id="cat_3" data-color="#c4a021" class="litab">
          税制改正
        </li>
        <li @click="changeTab" value="3" id="cat_4" data-color="#416ad3" class="litab">
          掲載情報
        </li>
        <li @click="changeTab" value="6" id="cat_5" data-color="#cccccc" class="litab">
          バックナンバー
        </li>
      </ul>
      <div class="c-tabs__content">
        <!-- All Posts - Display 5 Posts-->
        <ul id="all" class="c-listpost active">

          <li v-for="(item, index) in arr.slice(0,5)" :key="index" :index="index" class="c-listpost__item">
            <div class="c-listpost__info">
              <span class="datepost">{{ formateDate(item.date) }}</span>
              <span class="cat">
                <i class="c-dotcat" style="background-color: #1bb7c5"></i>

                <nuxt-link :to="{
                   name: 'new-cat-id',
                   //Truyền số bài post theo mỗi category
                   params: { posts: arr, id: cateNum, cateName: cateName }
                   }">
                  {{ cateName == [] ? cateName[index] : cateName }}
                </nuxt-link>

              </span>
            </div>
            <h3 class="titlepost">
              <nuxt-link :to="{
                  name: 'new-post-id', // đường dẫn tên của file và file.vue
                  params: { post: item, id: item.id, cateNum: cateNum, category: inforCategory}
                }">
                {{ item.title.rendered }}
              </nuxt-link>
            </h3>
          </li>

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
export default {
  name: 'News',
  components: {},
  props: {
    inforCategory: {
      type: Array,
      required: true
    },
    informationPost: {
      type: Array,
      required: true
    },
  },

  data() {
    return {
      category: this.inforCategory,
      cateNum: '',
      cateName: [],
      arr: this.informationPost,
    }
  },
  methods: {
    formateDate(date) {
      const str = date.trim()
      const y = str.substr(0, 4)
      const m = str.substr(5, 2)
      const d = str.substr(8, 2)
      return y + '年' + m + '月' + d + '日'
    },
    changeTab(event) {
      console.log(event)
      this.arr = []
      this.cateName = []
      const activeColor = event.target.dataset.color;
      this.cateNum = event.target.value;
      console.log('cateNum', this.cateNum)

      //Xóa style css cho tab
      const tablinks = document.getElementsByClassName('litab')
      for (let i = 0; i < tablinks.length; i++) {
        tablinks[i].classList.remove('active')
        tablinks[i].style.backgroundColor = ''
      }
      //Thêm style css cho tab được nhấn
      document.getElementById(event.target.id).classList.add('active')
      document.getElementById(event.target.id).style.backgroundColor = activeColor;

      //Làm mới mảng trong props
      this.informationPost.map((post, index) => {
        console.log('aaa', post.categories)
        let index1 = post.categories.findIndex(cate => cate === this.cateNum);
        console.log(index1);
        if (index1 !== -1) {
          this.arr = [...this.arr, post]
        }
        console.log('mang moi', this.arr)
      })

      //Xuất tên category
      if(this.cateNum == 9){
         this.informationPost.map((post, index)=>{
         let firstCate = post.categories[0]
         let i = this.inforCategory.findIndex(item => item.id == firstCate);
         this.cateName = [...this.cateName,this.inforCategory[i].name];
         console.log(this.cateName)
        })
      } else {
      let index = this.inforCategory.findIndex(item => item.id == this.cateNum);
      console.log('index', index)
      this.cateName = this.inforCategory[index].name;
      }
    }
  },
  mounted() {
    if(this.cateNum == ''){
        this.informationPost.map((post, index)=>{
        let firstCate = post.categories[0]
        let i = this.inforCategory.findIndex(item => item.id == firstCate);
        this.cateName = [...this.cateName,this.inforCategory[i].name];
        console.log(this.cateName)
      })
    }
  }
}
</script>
