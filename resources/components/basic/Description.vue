<template>
<div>
  <p v-for="(item2, idnex) in descript" class="c-checkMark">
    {{ item2["wp_steps-discription"] }}
  </p>
</div>
</template>

<script>
export default {
  name: 'Description',
  props: {
    descript: {
      type: Array,
      required: true
    }
  }
}
</script>
