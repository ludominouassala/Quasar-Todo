<template>
  <q-page class="bg-grey-3 colum">
    <div class="div.row q-pa-sm bg-primary">
      <q-input v-model="newTask" @keyup.enter="addTask" square filled bg-color="white" placeholder="Add Task" dense>
        <template v-slot:append>
          <q-btn @click="addTask" round dense flat icon="add" />
        </template>
      </q-input>
    </div>
    <q-list class="bg-white" separator borded>
     <q-item
      v-for="(task, index) in tasks" :key="task.title" @click="task.done = !task.done" :class="{'done bg-blue-1' : task.done}" clickable v-ripple>
      <q-item-section avatar>
        <q-checkbox v-model="task.done" class="no-pointer-events" color="primary"/>
      </q-item-section>
        <q-item-section>
          <q-item-label>{{task.title}}</q-item-label>
        </q-item-section>
        <q-item-section v-if= "task.done" side>
          <q-btn @click.stop="deleteTask(index)" flat round dense color="primary" icon="delete"/>
        </q-item-section>
      </q-item>
    </q-list>
    <div v-if="!tasks.length" class="no-tasks absolute-center">
      <q-icon name="check" size="100px" color="primary" />
      <div class="text-h5 text-primary text-center">No Tasks</div>
    </div>
  </q-page>
</template>
<script>
export default {
  data () {
    return {
      newTask: '',
      tasks: [ // { title: 'Eat Bananas', done: false }, { title: 'Poo Bananas', done: false }
      ]
    }
  },
  methods: {
    deleteTask (index) {
      this.$q.dialog({ title: 'Confirm', message: 'Really Delete?', cancel: true, persistent: true }).onOk(() => {
        this.tasks.splice(index, 1)
        this.$q.notify('Task deleted')
      })
    },
    addTask () {
      this.tasks.push({
        title: this.newTask,
        done: false
      })
      this.newTask = ''
    }
  }
}
</script>

<style lang="scss">
  .no-Tasks {opacity: 0.5}
</style>
