<template>
  <section id="main">
    <main>
      <Modal :activeModal="activeModal" title="Compras" buttonSave="Salvar" buttonCancel="Cancelar" @closeModal="activeModal = !activeModal">
        <form>
          <div class="columns">
            <div class="column is-10 field">
              <label class="label">Task</label>
              <input type="text" class="input w-100" />
            </div>
            <div class="column is-2">
              <label class="label op-0">Add</label>
              <input type="submit" class="button is-success w-100" value="Add">
            </div>
          </div>
          <hr/>
          <div class="is-flex is-justify-content-center is-align-content-center is-flex-wrap-wrap w-100">
            <div class="w-100 task-item p-3 mb-3 is-flex is-justify-content-space-between">
              <p class="is-bold is-flex is-align-items-center">Task 1</p>
              <div class="is-flex">
                <Button icon="edit" classButton="button is-info" class="pr-2"/>
                <Button icon="delete" classButton="button is-danger"/>
              </div>
            </div>
          </div>
        </form>
      </Modal>
      <div class="container mt-6 hv-100">
        <div class="w-100 is-flex is-justify-content-space-between pb-5">
          <form class="is-flex w-100 is-justify-content-space-between is-align-items-center">
            <label for="toDoListName" class="label">To do list Name</label>
            <input class="input w-100" v-model="todo.name" type="text" style="width: 65%"/>
            <Button classButton="button is-primary" icon="add" @action="createTodoList" style="width: 10%">Add Task</Button>
          </form>
        </div>
        <hr/>
        <div class="is-flex is-flex-wrap-wrap is-justify-content-space-between hv-100" v-if="todoList.length > 0">
          <div class="w-30" v-for="{id, name, tasks} in todoList" :key="id">
            <Button classButton="button is-primary" @action="openTodoList(id)">
              <TaskCard>
                <div>
                  <h1 class="title">{{ name }}</h1>
                  <h2 class="subtitle">{{ tasks.length }}</h2>
                </div>
              </TaskCard>
            </Button>
          </div>
        </div>
        <div class="is-flex is-flex-wrap-wrap is-justify-content-center pt-5" v-else>
          <div>
            <h1 class="title">No task registered!</h1>
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
        name: '',
        tasks: [],
      },
      todoList:[],
      task:{
        name:'',
        state:''
      },
      tasks: [],
    }
  },
  methods:{
    createTodoList(){
      let todo = {
        id: 1,
        name: this.todo.name,
        tasks: this.todo.tasks
      }
      this.todoList.push(todo)
    },
    openTodoList(id){
      this.activeModal = !this.activeModal
    }
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

</style>
