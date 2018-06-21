<template>
    <div>
        <div class="fixed-action-btn">
            <button class="btn-floating btn-large waves-effect waves-light red pulse modal-trigger" data-target="modal1" v-on:click="modalJs()"><i class="material-icons">add</i></button>
        
        </div>
        <div id="modal1" class="modal">
            <div class="modal-content">
                Add new Todo
                <div class="row">
                    <div class="input-field col s12">
                        <input type="text" id="task_name" v-model="task_name">
                        <label for="task_name">Task Name</label>
                    </div>
                    <div class="input-field col s12">
                        <input type="text" id="description" v-model="description" >
                        <label for="description">Description</label>
                    </div>
                    <div class="input-field col s12">
                        <input type="date" id="date">
                        <label for="date">Date</label>
                    </div>   
                </div>
            </div>
            <div class="modal-footer">
                <button class="modal-close waves-effect waves-green btn-flat" v-on:click="addTodo()">submit</button>
            </div>
        </div>

        <table>
        <thead>
          <tr>
              <th>Todo</th>
              <th>Description</th>
              <th>Due Date</th>
              <th>Options</th>
          </tr>
        </thead>

        <tbody v-for="todo in todoList">
          <tr>
            <td>{{todo.task_name}}</td>
            <td>{{todo.description}}</td>
            <td>$0.87</td>
            <td><button class="btn-floating btn-small waves-effect waves-light" v-on:click="deleteTodo"><i class="material-icons">edit</i></button>
            <button class="btn-floating btn-small waves-effect waves-light"><i class="material-icons">delete</i></button>
            
            </td>
          </tr>
        </tbody>
      </table>

    </div>
</template>

<script>
import axios from 'axios'

export default {
   created() {
       if(localStorage.hasOwnProperty("token")){
           this.getTodo()
       }else{
           this.$router.push("/")
       }
       
   },
   data() {
       return{
           todoList:[],
           task_name:"",
           description:""
       }
   },
    methods:{
        getTodo() {
            axios({
                method:"get",
                url:"http://localhost:3000/todos/show",
                headers:{
                   token: localStorage.getItem("token")
                }
            })
            .then(({data})=>{
                this.todoList = data
                console.log(data)
            })
        },
        modalJs() {
            var elems = document.querySelectorAll('.modal');
            var instances = M.Modal.init(elems);
        },
        addTodo() {
            axios({
                method:"post",
                url:"http://localhost:3000/todos/add",
                data:{
                    task_name:this.task_name,
                    description:this.description
                },
                headers: {
                    token: localStorage.getItem("token")
                }
            })
            .then(response=>{
                this.task_name=""
                this.description=""
                this.getTodo()
            })
        },
        deleteTodo() {
            
        }
    }
}


</script>

<style scoped>

</style>
