<template>
    <h3> Create a todo </h3>

    <h4> What on your todo list? </h4>
    <input @keyup="noTitle = false" class="text-dark" type="text" v-model="title" placeholder="e.g make a video">
    <span v-show="noTitle" class="error"> Enter a todo </span>

    <h4> Pick a category </h4>
    <div class="options">
        <label @click="noSelect = false">
            <input type="radio" name="category" value="business" v-model="category">
            <span class="bubble business"></span>
            <div class="option-title"> Business</div>
        </label>
        <label @click="noSelect = false">
            <input type="radio" name="category" value="personal" v-model="category">
            <span class="bubble personal"></span>
            <div class="option-title"> Personal</div>
        </label>
    </div>
    <span v-show="noSelect" class="error"> Select a category </span>

    <button @click="createTodo"> Add todo</button>

    <TodoList :todoList="todoList" @deleteTodo="deleteTodo" @editTodo="editTodo" @stopEdit="stopEdit"/>
</template>

<script>
import { ref } from '@vue/reactivity'
import TodoList from './TodoList.vue'

export default {
    components: { TodoList},

    setup(){

        const title = ref('');
        const category = ref('');

        let noSelect = ref(false);
        let noTitle = ref(false);

        const todos = ref(
            [
             {id: 1, title: "Create a website", category: "personal", edit: true},
             {id: 2, title: "Vue conference", category: "business", edit: true}
            ]
        )


        const createTodo = () => {
            if(!title.value){
                noTitle.value = true;
            }
            if(!category.value){
                noSelect.value = true
            }

            if(title.value && category.value){
                let id = Math.random() * 9999;
                const newTodo ={
                    id: id,
                    title: title.value,
                    category: category.value,
                    edit: true
                }

                todos.value.unshift(newTodo);

                reset();
            }
        }

        const deleteTodo =(id) => {
            todos.value = todos.value.filter( el => el.id != id);
        }

        const editTodo = (event, id) => {
            edit(id, false);
            let input = event.path[2].querySelector("input[type=text]");
            input.focus();
        }

        const stopEdit = (event,id) => {

            todos.value = todos.value.map( el =>
                el.id == id ? {...el, title: event.target.value} : el
            );
            edit(id, true);
        }

        const edit = (id, value) => {
            todos.value = todos.value.map( el =>
                el.id == id ? {...el, edit: value} : el
            );
        }

        const reset = () =>{
            title.value = '',
            category.value = '';
        }

        return {
            title: title,
            todoList : todos,
            createTodo,
            category: category,
            noSelect,
            noTitle,
            deleteTodo,
            editTodo,
            stopEdit
        }
    }
}
</script>

<style scoped>
    button {
        display: block;
        width: 100%;
        font-size: 1.125rem;
        padding: 1rem 1.5rem;
        color: #fff;
        background-color: #ea40a4;
        border-radius: 0.5rem;
        box-shadow: 0px 0px 4px  rgba(234, 64, 164, .75);
        cursor: pointer;
        transition: .2s ease-in-out;
    } 

    button:hover {
        opacity: 0.75;
    } 

    input[type=text] {
        display: block;
        width: 100%;
        font-size: 1.125rem;
        padding: 1rem 1.5rem;
        background-color: #fff;
        border-radius: 0.5rem;
        box-shadow: 0 1px 3px rgba(0,0,0,0.1);
        margin-bottom: .2rem;
    }

    .error {
        display: inline-block;
        font-size: .875rem;
        letter-spacing: 1px;
        font-weight: 700;
        color: #f00906;
        margin-bottom: 1.3rem;
    }

     .options {
        display: grid;
        grid-template-columns: repeat(2, 1fr);
        grid-gap: 1rem;
        margin-bottom: .5rem;
    }

    .options label{
       display: flex;
       flex-direction: column;
       align-items: center;
       justify-content: center;
       padding: 1.5rem;
       background: #fff;
       border-radius: 0.5rem;
       box-shadow: 0 1px 3px rgba(0,0,0,0.1);
       cursor: pointer;
    }

    .option-title {
        color: #313154;
        font-size: 1.125rem;
        margin-top: 1rem;
    }
</style>