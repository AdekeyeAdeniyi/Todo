<template>
  <div class="mt-4">
      <h3> Todo List </h3>
      <div class="todo-list">
          <div v-for="todo in todoList" :key="todo.id">
              <div v-if="todo.category == 'business'" class="todo-item">
                    <label>
                        <input type="checkbox" name="" id="">
                        <span @click="done($event)" class="bubble business"></span>
                    </label>

                    <div class="todo-content">
                        <input @change="stopEdit($event, todo.id)" type="text" :value="todo.title" :readonly="todo.edit"/>
                    </div>
                    <div class="actions">
                        <button @click="editTodo($event, todo.id)" class="edit"> Edit </button>
                        <button @click="deleteTodo(todo.id)" class="delete"> Delete</button>
                    </div>
              </div>
              <div v-else class="todo-item">
                    <label>
                        <input type="checkbox" name="" id="">
                        <span @click="done($event)" class="bubble personal"></span>
                    </label>

                    <div class="todo-content">
                        <input @change="stopEdit($event, todo.id)" type="text" :value="todo.title" :readonly="todo.edit"/>
                    </div>
                    <div class="actions">
                        <button @click="editTodo($event, todo.id)" class="edit"> Edit </button>
                        <button @click="deleteTodo(todo.id)" class="delete"> Delete</button>
                    </div>
              </div>
          </div>
      </div>
  </div>
</template>

<script>
import { ref } from '@vue/reactivity'
import { watch } from '@vue/runtime-core';
export default {
    props: ['todoList'],

    setup(_, context) {


        const deleteTodo = (id) =>{
            context.emit("deleteTodo", id);
        }

        const editTodo =(event, id) => {
            context.emit("editTodo", event, id)
        }

        const done = (event) => {
            let todoContainer = event.path[2];
            todoContainer.classList.toggle('done');
            todoContainer.querySelector('.edit').classList.toggle('disabled');
        }

       const stopEdit = (event, id) => {
           context.emit("stopEdit",event,id)
       }

        return {
            deleteTodo,
            editTodo,
            done,
            stopEdit
        }
    }
}
</script>

<style scoped>
   .todo-list .todo-item{
       display: flex;
       align-items: center;
       background-color: #fff;
       padding: 1rem;
       border-radius: 0.5rem;
       box-shadow:  0 1px 3px rgba(0, 0, 0, 0.1);
       margin-bottom: 1rem;
    }

    .todo-item label{
        display: block;
        margin-right: 1rem;
        cursor: pointer;
    }

    .todo-item .todo-content{
        flex: 1 1 0%;
    }

    .todo-item .todo-content input{
        color: #313154;
        font-size: 1.125rem;
        text-transform: capitalize;
    }
    
    .todo-item .actions {
        display: flex;
        align-items: center;
    }

    .todo-item .actions button{
        display: block;
        padding: 0.5rem;
        border-radius: 0.25rem;
        color: #fff;
        cursor: pointer;
        transition: .2s ease-in-out;
    }

    .todo-item .actions button:hover{
        opacity: 0.75;
    }

    .todo-item .actions .edit{
       margin-right: 0.5rem;
       background-color: #ea40a4;
    }

    .todo-item .actions .edit.disabled{
        opacity: 0.4;
        cursor: not-allowed;
        pointer-events: all;
    }

    .todo-item .actions .delete{
       background-color: #ff5b57;
    }

    .todo-item.done .todo-content input{
       text-decoration: line-through;
       color: #8888;
    }
</style>