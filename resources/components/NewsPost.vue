<template>
<div>
  <li v-for="(item, index) in arr.slice(0,5)" :key="index" :index="index" class="c-listpost__item">
    <div class="c-listpost__info">
      <span class="datepost">{{ formateDate(item.date) }}</span>
      <span class="cat">
        <i class="c-dotcat" v-bind:style="{ 'background-color': setColor()}"></i>
        <nuxt-link :to="{
            name: 'new-cat-id',
            // Xét cateName có phải là mảng hay không
            //Truyền số bài post theo mỗi category
            params: { posts: arr, id: cateNum, cateName: cateName }
            }">
          {{ Array.isArray(cateName) ? cateName[index] : cateName.name }}
        </nuxt-link>
      </span>
    </div>
    <h3 class="titlepost">
      <nuxt-link :to="{
        name: 'new-post-id', // đường dẫn tên của file và file.vue
        params: { post: item, id: item.id, cateNum: cateNum, category: category}
        }">
        {{ item.title.rendered }}
      </nuxt-link>
    </h3>
  </li>
</div>
</template>

<script>
export default {
  name: 'NewsPost',
  data() {
    return {
      colors:[
        {id:0,color:'#1bb7c5'},
        {id:7,color:'#1bb7c5'},
        {id:6,color:'#ccc'},
        {id:3,color:'#416ad3'},
        {id:5,color:'#c4a021'},
        {id:4,color:'#d6772a'}
      ]
    }
  },
  props: {
    arr: {
      type: Array,
      required: true
    },
    category: {
      type: Array,
      required: true
    },
    cateName: {
      required: true
    },
    cateNum: {
      required: true
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
    setColor(){
      let index = this.colors.findIndex(cl => cl.id == this.cateNum);
      if(index !== -1){
        return this.colors[index].color
      }

    }
  }
}
</script>
