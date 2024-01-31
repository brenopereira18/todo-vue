<script setup>
import { reactive } from 'vue';

  const estado = reactive({
    filtro: 'todas',
    novaTarefa: '',
    tarefas: [
      {
        titulo: 'Estudar ES6',
        finalizada: false
      },
      {
        titulo: 'Estudar SASS',
        finalizada: false
      },
      {
        titulo: 'Ir para a academia',
        finalizada: true
      },
    ]
  })

  const getTarefasPendentes = () => {
    return estado.tarefas.filter(tarefa => !tarefa.finalizada)
  }

  const getTarefasFinalizadas = () => {
    return estado.tarefas.filter(tarefa => tarefa.finalizada)
  }

  const getTarefasFiltradas = () => {
    const { filtro } = estado

    switch (filtro) {
      case 'pendentes':
        return getTarefasPendentes()
      case 'finalizadas':
        return getTarefasFinalizadas()
      default:
        return estado.tarefas    
    }
  } 
  
  const CadastrarTarefa = () => {
    const tarefaNova = {
      titulo: estado.novaTarefa,
      finalizada: false
    }
    estado.tarefas.push(tarefaNova)
    estado.novaTarefa = ''
  }
  
</script>

<template>
  <div class="container">
    <header class="p-5 mb-4 mt-4 bg-light rounded-3">
      <h1>Minhas tarefas</h1>
      <p>
        Você possui {{ getTarefasPendentes().length }} tarefas pendentes
      </p>
    </header>
    <form @submit.prevent="CadastrarTarefa">
      <div class="row">
        <div class="col">
          <input :value="estado.novaTarefa" @change="event => estado.novaTarefa = event.target.value" type="text" placeholder="Digite aqui a descrição da tarefa" class="form-control">
        </div> 
        <div class="col-md-2">
          <button type="submit" class="btn btn-primary">Cadastrar</button>
        </div>
        <div class="col-md-2">
          <select @change="event => estado.filtro = event.target.value" class="form-control">
            <option value="todas">Todas tarefas</option>
            <option value="pendentes">Pendentes</option>
            <option value="finalizadas">Finalizadas</option>
          </select>
        </div>
      </div>
    </form>
    <ul class="list-group mt-4">
      <li class="list-group-item" v-for="tarefa in getTarefasFiltradas()">
        <input @change="event => tarefa.finalizada = event.target.checked" :checked="tarefa.finalizada" :id="tarefa.titulo" type="checkbox">
        <label :class="{ done: tarefa.finalizada }" :for="tarefa.titulo" class="ms-3">{{ tarefa.titulo }}</label>
      </li>
    </ul>
  </div>
  
</template>
  
<style scoped>
  .done {
    text-decoration: line-through;
  }
</style>
