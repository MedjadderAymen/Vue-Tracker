<template>
  <header>
    <h1>
      {{ title }}
    </h1>
    <Button v-show="homePage" :text="showAddTask ? 'close' : 'Add' " :color="showAddTask ? 'red' : 'green' "
            @btn-click="$emit('toggle-add-task')"/>
  </header>
</template>

<script>

import Button from './Button.vue'

export default {
  name: 'Header',
  //props: ['title']
  /*props: {
      title: String,
  },*/
  props: {
    title: {
      type: String,
      default: "Hello world"
    },
    showAddTask: Boolean
  },
  components: {
    Button
  },
  computed: {
    homePage() {
      if (this.$route.path === '/') {
        return true
      } else {
        false
      }
    }
  }
}
</script>

<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}
</style>