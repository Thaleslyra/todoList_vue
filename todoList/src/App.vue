<script setup>
import { reactive } from 'vue';
const estado = reactive({

  filtro: 'todas',
  tarefaTemp: ' ',



  tarefas : [
    {
      titulo: 'Estudar Es6',
      finalizada: false,
    },

    {
      titulo: 'Estudar Sass',
      finalizada: false,
    },

    {
      titulo: 'Ir para academia',
      finalizada: true
    }
  ]

})

const getTarefasPendentes = () => {
   return estado.tarefas.filter(tarefa => !tarefa.finalizada)
}

const getTarefasFinalizadas = () => {
  return  estado.tarefas.filter (tarefa => tarefa.finalizada )
}

const getTarefasFiltradas = () => {

  const {filtro} = estado;

  switch(filtro) {

    case 'pendentes':
      return getTarefasPendentes();
      
      case 'finalizadas':
        return getTarefasFinalizadas();

        default:
          return estado.tarefas;
  }

}

const cadastraTarefa = () => {

  const tarefaNova = {
    titulo: estado.tarefaTemp,
    finalizada: false,
  }

  estado.tarefas.push(tarefaNova);
  estado.tarefaTemp = ' ';


}

</script>

<template>
<div class="container">
  <header class="p-5 mb-4 mt-4 bg-dark rounded-3">
    <h1 class=" text-light">Minhas tarefas</h1>
    <p class="text-light">** Você possui {{ getTarefasPendentes().length }} tarefas pendentes</p>
  </header>

  <div class="mb-4">
        <select name="" id="" class="formcontrol" @change="evento => estado.filtro = evento.target.value">
         <option value="todas">Todas as tarefas</option>
         <option value="pendentes">Tarefas pendentes</option>
         <option value="finalizadas">Tarefas finalizadas</option>
        </select>
      </div>
     


  <form @submit.prevent="cadastraTarefa">
    <div class="row">
      <div class="col">
        <input :value="estado.tarefaTemp" @change="evento => estado.tarefaTemp = evento.target.value" required type="text" placeholder="Digite aqui a descrição da tarefa" class="form-control">
      </div>
      <div class="col-md-2">
        <button class="btn btn-primary">Cadastrar</button>
      </div>   
    </div>
  </form>
  

  <ul class="list-group mt-4">
    <li class="list-group-item" v-for="tarefa in getTarefasFiltradas()">
      <input @change="evento => tarefa.finalizada = evento.target.checked" :checked="tarefa.finalizada" :id="tarefa.titulo" type="checkbox">
      <label :for="tarefa.titulo" class="ms-3" :class="{done: tarefa.finalizada}">
        {{ tarefa.titulo }}
      </label>
    </li>
  </ul>


</div>
</template>

<style scoped>

.done {
  text-decoration: line-through;
}

</style>
