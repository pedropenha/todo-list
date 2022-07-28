<template>
  <section id="main">
    <main>
      <Modal :activeModal="activeModal" :title="todo.name" buttonSave="Salvar" buttonCancel="Cancelar" @closeModal="activeModal = !activeModal">
        <form @submit.prevent="insertTask()">
          <div class="columns">
            <div class="column is-10 field">
              <label class="label">Task</label>
              <input type="text" class="input w-100" v-model="task.name"/>
            </div>
            <div class="column is-2">
              <label class="label op-0">Add</label>
              <input type="submit" class="button is-success w-100" value="Add">
            </div>
          </div>
          <hr/>
          <div v-if="tasks.length > 0">
            <div class="is-flex is-justify-content-center is-align-content-center is-flex-wrap-wrap w-100" v-for="{id, idTodo, content} in tasks" :key="id" v-if="tasks.length > 0">
            <div class="w-100 task-item p-3 mb-3 is-flex is-justify-content-space-between">
              <p class="is-bold is-flex is-align-items-center w-60" :id="id+'p'">{{ content }}</p>
              <div class="is-hidden is-justify-content-space-between w-100" :id="id+'i'">
                <input class="input w-60" v-model="newContentTask"/>
                <div class="is-flex is-justify-content-flex-end w-40">
                  <Button classButton="button is-success ml-5" icon="save" @action="updateTaskName(id, idTodo)"></Button>
                  <Button classButton="button is-danger ml-5" icon="cancel" @action="cancelActionTask(id)"></Button>
                </div>
              </div>
              <div class="is-flex w-40 is-justify-content-space-between" :id="id+'d'">
                  <Button icon="check" classButton="button is-success" @action="concludeTask(id)"/>
                  <Button icon="edit" classButton="button is-info" @action="editTask(id, content)"/>
                  <Button icon="delete" classButton="button is-danger" @action="deleteTask(id, content)"/>
                </div>
              </div>
            </div>
          </div>
          <div class="is-flex is-flex-wrap-wrap is-justify-content-center" v-else>
            <div>
              <h2 class="subtitle">No task registered!</h2>
            </div>
          </div>
        </form>
      </Modal>
      <div class="container mt-6 hv-100">
        <div class="w-100 is-flex is-justify-content-space-between pb-5">
          <form class="is-flex w-100 is-justify-content-space-between is-align-items-center" @submit.prevent="insertTodo">
            <label for="toDoListName" class="label">Todo list Name</label>
            <input class="input w-100" v-model="todo.name" type="text" style="width: 65%"/>
            <button class="button is-primary" type="submit">
              Add Task
              <span class="material-icons">add</span>
            </button>
          </form>
        </div>
        <hr/>
        <div class="is-flex is-flex-wrap-wrap is-justify-content-space-between hv-100" v-if="todoList.length > 0">
          <div class="w-30" v-for="{id, title} in todoList" :key="id">
            <TaskCard>
              <div class="w-100">
                <h1 class="title w-100" :id="id">{{ title }}</h1>
                <div class="is-hidden" :id="id+'i'">
                  <input class="input" v-model="newTitleTodo"/>
                  <Button classButton="button is-success ml-5" icon="save" @action="updateTodoListName(id)"></Button>
                  <Button classButton="button is-danger ml-5" icon="cancel" @action="cancelAction(id)"></Button>
                </div>
              </div>
              <div class="is-flex is-justify-content-space-between is-align-items-center pt-5 w-100">
                <Button @action="openTodoList(id, title)" classButton="button is-success" icon="folder_open"></Button>
                <Button @action="editTodoName(id, title)" classButton="button is-link" icon="edit"></Button>
                <Button @action="deleteTodoList(id, title)" classButton="button is-danger" icon="delete"></Button>
              </div>
            </TaskCard>
          </div>
        </div>
        <div class="is-flex is-flex-wrap-wrap is-justify-content-center pt-5" v-else>
          <div>
            <h1 class="title">No todo list registered!</h1>
          </div>
        </div>
      </div>
    </main>
  </section>
</template>

<script>
export default{
  data(){
    return{
      activeModal: false,
      todo:{
        id: '',
        name: ''
      },
      todoList:[],
      task:{
        name:'',
        state:''
      },
      tasks: [],
      editTaskName: false,
      newTitleTodo: '',
      newContentTask: ''
    }
  },
  methods:{
    deleteTask(id, content){
      if(!confirm(`Delete ${content} task?`))
        return false

      this.$axios.delete('http://localhost:8080/task/'+id)

      this.tasks.map((task, index) => {
        if(task.id === id){
          this.tasks.splice(index, 1)
        }
      })
    },

    async openTodoList(id, title){
      this.activeModal = !this.activeModal

      this.todo.name = title
      this.todo.id = id

      const response = await this.$axios.get('http://localhost:8080/task/taskByIdTodo/'+id)

      this.tasks = response.data.tasks
    },

    insertTodo(){
      this.$axios.post('http://localhost:8080/todoList', {title: this.todo.name}).then(async () => {
        const response = await this.$axios.get('http://localhost:8080/todoList')
        this.todoList = response.data
      }).catch((e) => {
        console.log(e)
      })
    },

    deleteTodoList(id, title){
      if(!confirm(`Delete ${title} todo list`))
        return false

      this.$axios.delete('http://localhost:8080/todoList/'+id)

      this.todoList.map((todoList, index) => {
        if(todoList.id === id){
          this.todoList.splice(index, 1)
        }
      })

    },

    editTodoName(id, title){
      document.getElementById(id).hidden = true
      document.getElementById(id+'i').classList.add('is-flex')
      document.getElementById(id+'i').classList.remove('is-hidden')
      this.newTitleTodo = title
    },

    editTask(id, content){
      document.getElementById(id+'p').classList.add('is-hidden')
      document.getElementById(id+'d').classList.add('is-hidden')
      document.getElementById(id+'d').classList.remove('is-flex')

      document.getElementById(id+'i').classList.add('is-flex')
      document.getElementById(id+'i').classList.remove('is-hidden')
      this.newContentTask = content
    },

    cancelActionTask(id){
      document.getElementById(id+'p').classList.remove('is-hidden')
      document.getElementById(id+'d').classList.add('is-flex')
      document.getElementById(id+'d').classList.remove('is-hidden')

      document.getElementById(id+'i').classList.add('is-hidden')
      document.getElementById(id+'i').classList.remove('is-flex')
      this.newContentTask = ''
    },

    updateTodoListName(idTodo){
      this.$axios.put('http://localhost:8080/todoList', {title: this.newTitleTodo, id: idTodo}).then(async () => {
        const response = await this.$axios.get('http://localhost:8080/todoList')
        this.todoList = response.data
      })
      this.cancelAction(idTodo)
    },

    updateTaskName(idTask, idTodo){
      this.$axios.put('http://localhost:8080/task', {id: idTask, content: this.newContentTask}).then(async () => {
        const response = await this.$axios.get('http://localhost:8080/task/taskByIdTodo/'+idTodo)
        console.log(response.data)
        this.tasks = response.data.tasks
      })
      this.cancelActionTask(idTask)
    },

    cancelAction(id){
      this.newTitleTodo = ''
      document.getElementById(id).hidden = false
      document.getElementById(id+'i').classList.add('is-hidden')
      document.getElementById(id+'i').classList.remove('is-flex')
    },

    insertTask(){
      this.$axios.post('http://localhost:8080/task', {idTodo: this.todo.id, content: this.task.name}).then(async () => {
        const response = await this.$axios.get('http://localhost:8080/task/taskByIdTodo/'+this.todo.id)
        this.tasks = response.data.tasks
        this.task.name = ''
      }).catch((e) => {
        console.log(e)
      })
    },
  },
  async created() {
    const response = await this.$axios.get('http://localhost:8080/todoList')
    this.todoList = response.data
  }
}
</script>

<style scoped>
#main{
  overflow: hidden;
}

.w-100{
  width: 100%;
}

.w-30{
  width: 30%;
}

.hv-100{
  height: 100vh;
}

.op-0{
  opacity: 0;
}

.task-item{
  border: 0;
  border-top: 1px solid #c3c3c3;
  border-bottom: 1px solid #c3c3c3;
}

.task-item:hover{
  background-color: #f2f2f2;
}

.button{
  transition: all .5s ease;
}

.button:hover{
  box-shadow: rgba(0, 0, 0, 0.24) 0 3px 8px;
}

.w-40{
  width: 40%;
}

.w-60{
  width: 60%;
}

</style>
