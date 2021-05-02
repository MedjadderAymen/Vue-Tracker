<template>
  <div :key="task.id" v-for="task in tasks">
    <Task :task="task" @toggle-reminder="$emit('toggle-reminder', task.id)"
          @delete-task="$emit('delete-task',task.id)"/>
  </div>
</template>

<script>

import Task from './Task'

export default {
  name: 'Tasks',
  props: {
    tasks: Array
  },
  components: {
    Task,
  },
  emits: ['delete-task', 'toggle-reminder']
}
</script>