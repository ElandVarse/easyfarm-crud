<template>
    <div class="app">
        <h1>Easyfarm <span>CRUD</span></h1>

        <!-- Info insertion -->
        <form class="addTodo"> 
            <fieldset>
                <input type="text" id="projeto" v-model="project" placeholder="Projeto">
                <input type="text" id="name"    v-model="name" placeholder="Nome">
                <input type="text" id="status"  v-model="status" placeholder="Status">
                <button @click="addTodo">Adicionar</button>
            </fieldset>
        </form>

        <!-- Todo List -->
        <table class="todo" v-if="todos.length>0"> 
            <tr>
                <th>Projeto</th>
                <th>Nome</th>
                <th>Status</th>
                <th style="background-color: #2f66cd">Editar</th>
                <th style="background-color: #ff2525">Apagar</th>
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
                <h2>Editar atividade</h2>
                <input type="text"  id="editProject" v-model="editData.project" :placeholder="todos[idToChange].project"><br>
                <input type="text"  id="editName"    v-model="editData.name"    :placeholder="todos[idToChange].name"><br>
                <input type="text"  id="editStatus"  v-model="editData.status"  :placeholder="todos[idToChange].status"><br>
                <button @click="doneEditing" style="margin-top: 16px">Confirmar</button>
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

                editData:{
                    name: '',
                    project: '',
                    status: '',
                }
            }
                   
        },

        created(){
            this.todos = JSON.parse(localStorage.getItem(STORAGE_KEY)  || '[]')
        },

        methods: {
            addTodo(){
                if(this.name.length<=0 || this.project.length<=0 || this.status.length<=0){
                    alert("Por favor, preencha o formulário corretamente.")
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
                if(this.editData.name.length<=0){
                    this.todos[this.idToChange].name;
                }
                else{
                    this.todos[this.idToChange].name = this.editData.name;
                }
                if(this.editData.project.length<=0){
                    this.todos[this.idToChange].project;
                }
                else{
                    this.todos[this.idToChange].project = this.editData.project;
                }
                if(this.editData.status.length<=0){
                    this.todos[this.idToChange].status;
                }
                else{
                    this.todos[this.idToChange].status  = this.editData.status;
                }
                
                localStorage.setItem(STORAGE_KEY, JSON.stringify(this.todos));

                this.editData.name = ''
                this.editData.project = ''
                this.editData.status = ''
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
