<script setup lang="ts">
import axios, { AxiosResponse } from 'axios';
import {ref} from 'vue';
import 'bootstrap/dist/css/bootstrap.min.css'; // Importando o Bootstrap

const newTodo = ref('');
const todos = ref()


axios.get('todos')
.then((response: AxiosResponse) => {
  todos.value = response.data
 
})

function updateTodo(todo: any) {
  const data = {
    completed: !todo.completed
  }
  axios.patch(`todos/${todo.id}`, data)
  .then((response: AxiosResponse) => {
    todo.completed = response.data.completed
  })
}
 
async function deleteTodo(id: number) {
 await axios.delete(`todos/${id}`)
 .then(() => {
  todos.value = todos.value.filter((todo: any) => todo.id !== id);
 } )
 

}

function addTodo(){
  if(newTodo.value.trim() !== ''){

    const data = {
      title: newTodo.value,
      completed: false
    };
    axios.post('todos', data)
    .then((response: AxiosResponse) => {
      todos.value.push(response.data);
      newTodo.value = '';
    });
  }
}

function teclarEnter(event: KeyboardEvent) {
  if (event.key === 'Enter') {
    addTodo();
  }
}


</script>

<template>

<div class="mb-5 row text-center">
  <h1>Lista de tarefas - TODO LIST</h1>
</div>


  <div class="row bg-white rounded shadow-sm p-2">
            <div class="col-md">
                <input class="form-control border-0 bg-transparent" type="text" v-model="newTodo" @keydown="teclarEnter" placeholder="Digite sua tarefa...">
            </div>
            <div class="col-auto px-0 mx-0 mr-2">
                <button @click="addTodo"  type="button" class="btn btn-secondary">Criar tarefa</button>
            </div>
  </div>
  
  <div class="container">
    <div class="d-flex row gap-2 mt-5">
        <div 
          class="d-flex align-items-center justify-content-between p-2"
          v-for="todo in todos"
          :key="todo.id"
          :class="{ 'border border-success border-3': todo.completed == true, 'border border-danger border-3': todo.completed == false }">
            <input type="checkbox" :id="'todo-' + todo.id "  v-model="todo.completed" @click="updateTodo(todo)">
            {{ todo.title }}

            
            
            <div class="">
              <button class="btn btn-danger" @click="deleteTodo(todo.id)">Deletar</button>
  
            </div>
            
          </div>
    </div>
    
         

          

  </div>


  
</template>

<style scoped>
</style>
