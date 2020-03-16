<template>
<div>
  <div v-for="(item1, index) in subTitle" :key="index" :index="index">

      <h5 class="flow-subtitle"> {{ item1["wp_steps-subtitle"] }} </h5>

      <description :descript="item1['wp_steps-discription']">
      </description>
  </div>
</div>
</template>

<script>
import Description from '@/components/basic/Description'
export default {
  name: 'Service',
  components: {
    Description
  },
  props: {
    subTitle: {
      type: Array,
      required: true
    }
  }
}
</script>
