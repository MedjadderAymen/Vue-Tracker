<template>

  <AddTask @add-task="addTask" v-show="showAddTask"/>

  <Tasks :tasks="tasks" @delete-task="deleteTask" @toggle-reminder="toggleReminder"/>
</template>

<script>

import Tasks from '../components/Tasks'
import AddTask from '../components/AddTask'

export default {
  name: 'Home',
  props: {
    showAddTask: Boolean
  },
  components: {
    Tasks,
    AddTask
  },
  data() {
    return {
      tasks: [],
    }
  },
  methods: {
    /*deleteTask(id){
      if(confirm('are you sure !')){
        this.tasks = this.tasks.filter((task)=> task.id !== id )
      }
    },*/
    async deleteTask(id) {
      if (confirm('are you sure !')) {

        const res = await fetch(`api/tasks/${id}`, {
          method: 'DELETE'
        })

        res.status === 200 ? (this.tasks = await this.fetchTasks()) : alert('Error deleting task')

      }
    },
    /*toggleReminder(id){
      this.tasks = this.tasks.map((task)=> task.id == id ? {...task, reminder : !task.reminder} : task)
    },*/
    async toggleReminder(id) {

      const taskToToggle = await this.fetchTask(id)

      const updateTask = {...taskToToggle, reminder: !taskToToggle.reminder}

      const res = await fetch(`api/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(updateTask)
      })

      const data = await res.json();

      this.tasks = this.tasks.map((task) => task.id == id ? {...task, reminder: data.reminder} : task)
    },
    /*addTask(task){
      this.tasks = [...this.tasks, task]
    },*/
    async addTask(task) {

      const res = await fetch('api/tasks', {
        method: 'POST',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(task)
      })

      const data = await res.json()

      this.tasks = [...this.tasks, data]
    },
    async fetchTasks() {

      const res = await fetch('api/tasks')

      const data = await res.json()

      return data

    },
    async fetchTask(id) {

      const res = await fetch(`api/tasks/${id}`)

      const data = await res.json()

      return data

    }
  },
  async created() {
    /*this.tasks = [
      {
        id:1,
        text: 'Doctors appointment',
        day:'March 1st at 2.30pm',
        reminder:true
      },
      {
        id:2,
        text: 'Meeting at school',
        day:'March 3rd at 1.30pm',
        reminder:true
      },
      {
        id:3,
        text: 'Food shopping',
        day:'March 3rd at 11.00am',
        reminder:false
      }
    ]*/

    this.tasks = await this.fetchTasks();
  },
}
</script>