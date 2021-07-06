<template>
    <div class="app">
        <h1>Vue <span>CRUD</span></h1>

        <!-- Info insertion -->
        <form class="addTodo"> 
            <fieldset>
                <input type="text" id="projeto" v-model="project" placeholder="Project">
                <input type="text" id="name"    v-model="name" placeholder="Name">
                <input type="text" id="status"  v-model="status" placeholder="Status">
                <button @click="addTodo">Add</button>
            </fieldset>
        </form>

        <!-- Todo List -->
        <table class="todo" v-if="todos.length>0"> 
            <tr>
                <th>Project</th>
                <th>Name</th>
                <th>Status</th>
                <th style="background-color: #2f66cd">Edit</th>
                <th style="background-color: #ff2525">Delete</th>
            </tr>

            <tr v-for="todo in todos" :key="todo.id"> 
                <td>{{todo.project}}</td>
                <td>{{todo.name}}</td>
                <td>{{todo.status}}</td>
                <td @click="editTodo(todo)"  ><img src="./assets/img/edit.png"   alt="Edit" width="24px" style="cursor: pointer"></td>
                <td @click="removeTodo(todo)"><img src="./assets/img/delete.png" alt="Edit" width="24px" style="cursor: pointer"></td>
            </tr>
        </table>

        <!-- Edit todo - Modal -->
        <transition name="fade" appear>
            <div class="modal-overlay" v-if="showModal" @click="showModal = false"></div>
        </transition>

        <transition name="fade" appear>
            <form class="modal" v-if="showModal">
                <h2>Edit</h2>
                <input type="text"  id="editProject" v-model="project" :placeholder="todos[idToChange].project">
                <input type="text"  id="editName"    v-model="name"    :placeholder="todos[idToChange].name">
                <input type="text"  id="editStatus"  v-model="status"  :placeholder="todos[idToChange].status">
                <button @click="doneEditing" style="margin-top: 16px">Done!</button>
            </form>
        </transition>
    </div>
</template>

<script>
    const STORAGE_KEY = 'todo-storage'
    export default {
        name: 'App',    

        data(){
            return{
                showModal: false,
                name: '',
                status: '',
                project: '',
                
                todos:[],

                idToEdit: '',
            }
        },

        created(){
            this.todos = JSON.parse(localStorage.getItem(STORAGE_KEY)  || '[]')
        },

        methods: {
            addTodo(){
                if(this.name.length<=0 || this.project.length<=0 || this.status.length<=0){
                    alert("Please, fill the form correctly.")
                }
                else{
                    this.todos.push({
                        name:   this.name,
                        project:this.project,
                        status: this.status,
                        id:     this.todos.length,
                    })
                    localStorage.setItem(STORAGE_KEY, JSON.stringify(this.todos));
                }
            },

            removeTodo(todo){
                this.todos.splice(this.todos.indexOf(todo), 1);
                localStorage.setItem(STORAGE_KEY, JSON.stringify(this.todos));
            },

            editTodo(todo){
                this.showModal = true
                this.idToChange = todo.id
            },

            doneEditing(){
                this.showModal = false

                // Validations
                if(this.name.length<=0){
                    this.todos[this.idToChange].name;
                }
                else{
                    this.todos[this.idToChange].name = this.name;
                }
                if(this.project.length<=0){
                    this.todos[this.idToChange].project;
                }
                else{
                    this.todos[this.idToChange].project = this.project;
                }
                if(this.status.length<=0){
                    this.todos[this.idToChange].status;
                }
                else{
                    this.todos[this.idToChange].status  = this.status;
                }
                
                localStorage.setItem(STORAGE_KEY, JSON.stringify(this.todos));

                this.name = ''
                this.project = ''
                this.status = ''
            }
        }
    }
</script>

<style>
    @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700&display=swap');
    @import url(./assets/table.css);
    @import url(./assets/form.css);
    @import url(./assets/modal.css);
    
    *{
        margin: 0;
        padding: 0;
        box-sizing: border-box;
        outline: none;
        border:none;
    }

    .app {
    font-family: 'Roboto', sans-serif;
    text-align: center;
    height: 100%;
    width: 100vw;

    margin-top: 120px;

    display: flex;
    flex-direction: column;
    align-items: center;
}

    h1{
        color: #333;
        font-size: 42px;
        margin-bottom: 60px;
    }

    h2{
        margin-bottom: 24px;
    }

    span{
        color: green
    }

    h2{
        color: #326ada;
        font-size: 32px;
    }

    button{
        font-size: 16px;
        margin-left: 8px;
        margin-top: 16px;
        padding: 8px 16px;
        color: #f9f9f9;
        background: #326ada;
        font-weight: bold;
        box-shadow: 3px 3px 8px #00000055;
        cursor: pointer;
        transition: .2s;
    }

    button:hover{
        background: #2855af;
    }
</style>
