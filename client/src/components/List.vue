<template>
  <div class="page-wrapper hello">
    <div id="todo-list-example" class="container">
      <div class="row">
        <div class="col-md-6 mx-auto">
          <h1 class="text-center" style="margin-top: 20px">TO-DO List App</h1>
          <form v-on:submit.prevent="addNewTask">
            <label for="tasknameinput">Task Name</label>
            <input v-model="taskname" id="tasknameinput" class="form-control" placeholder="Add New Task">
            <button v-if="this.isEdit == false" type="submit" class="btn btn-success btn-block  mt-3">
              Submit
            </button>
            <button v-else type="button" v-on:click="updateTask()" class="btn btn-primary btn-block  mt-3">
              Update
            </button>
          </form>

          <transition-group tag="table" class="table" name="animate-todo" enter-active-class="animated fadeInUp" leave-active-class="animated fadeOutDown">
            <tr v-for="(todo) in todos" v-bind:key="todo.id" v-bind:title="todo.title">
              <td class="text-left"><button v-on:click="show = !show" class="nobutton">{{todo.title}}</button>
                <transition name="fade">
                  <p style="padding-left: 20px" v-show="show">Deskripsi</p>
                </transition>
              </td>
              <td class="text-right">
                <button v-on:click="editTask(todo.title, todo.id)" class=" btn btn-info ">Edit</button>
                <button v-on:click="deleteTask(todo.id)" class=" btn btn-danger ">Delete</button>
              </td>
            </tr>
          </transition-group>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data () {
    return {
      todos: [],
      id: '',
      taskname: '',
      isEdit: false,
      show: false
    }
  },
  mounted () {
    this.getTasks()
  },
  methods: {
    getTasks () {
      axios({ method: 'GET', url: '/api/tasks' }).then(
        result => {
          console.log(result.data)
          this.todos = result.data
        },
        error => {
          console.error(error)
        }
      )
    },
    addNewTask () {
      axios.post('/api/tasks',
        { title: this.taskname }
      ).then((res) => {
        this.taskname = ''
        this.getTasks()
        console.log(res)
      }).catch((err) => {
        console.log(err)
      })
    },

    editTask (title, id) {
      this.id = id
      this.taskname = title
      this.isEdit = true
    },

    updateTask () {
      axios.put(`/api/tasks/${this.id}`,
        { title: this.taskname }
      ).then((res) => {
        this.taskname = ''
        this.isEdit = false
        this.getTasks()
        console.log(res)
      }).catch((err) => {
        console.log(err)
      })
    },

    deleteTask (id) {
      axios.delete(`/api/tasks/${id}`
      ).then((res) => {
        this.taskname = ''
        this.getTasks()
        console.log(res)
      }).catch((err) => {
        console.log(err)
      })
    }
  }
}
</script>

<style scoped>
  .fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
  }

  .fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
    opacity: 0;
  }

  .nobutton {
    background-color: Transparent;
    background-repeat:no-repeat;
    border: none;
    cursor:pointer;
    overflow: hidden;
    outline:none;
    font-weight: 700;
  }
</style>
