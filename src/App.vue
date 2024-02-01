<script setup>
  import { reactive } from 'vue';
  import Cabecalho from './components/Cabecalho.vue'
  import Formulario from './components/Formulario.vue'
  import ListaDeTarefas from './components/ListaDeTarefas.vue'

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
  
  const cadastrarTarefa = () => {
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
    <Cabecalho :tarefas-pendentes="getTarefasPendentes().length" v-if="getTarefasPendentes().length > 0"/>
    <h3 class="p-5 mb-4 mt-4 bg-light rounded-3" v-else>Você não possui tarefas pendentes</h3>       
    <Formulario :nova-tarefa="estado.novaTarefa" :edita-nova-tarefa="event => estado.novaTarefa = event.target.value" :cadastra-tarefa="cadastrarTarefa" :trocar-filtro="event => estado.filtro = event.target.value"/>
    <ListaDeTarefas :tarefas="getTarefasFiltradas()"/> 
  </div>  
</template>
  

