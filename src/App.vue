<script setup>
import { reactive } from 'vue';


const estado = reactive({
  filtro: 'todas',
  tarefatemp: '',

  tarefas: [{
    titulo: 'Estudar ES6',
    finalizada: false
    },
    {
      titulo: 'Estudar SASS',
      finalizada: false
    },
    {
      titulo: 'Ir para academia',
      finalizada: true
    }
  ]
})

const getTarefaspendentes = () => {
  return estado.tarefas.filter(tarefa => !tarefa.finalizada )
}

const getTarefasFinalizadas = () => {
  return estado.tarefas.filter(tarefa => tarefa.finalizada)
}

const getTarefasFiltradas = () => {
  const {filtro} = estado;

  switch (filtro){
    case 'pendentes': return getTarefaspendentes();
    case 'finalizadas': return getTarefasFinalizadas();
    default: return estado.tarefas;
  }
}
const cadastratarefa = () => {

    const tarefaNova = {
      titulo: estado.tarefatemp,
      finalizada: false
    }
    estado.tarefas.push(tarefaNova)
    estado.tarefatemp = '';
}

</script>





<template>
  <div class="container">

    <header class="p-5 mb-4 mt-4 bg-light rounded-3">
      <h1> Minhas Tarefas</h1>
      <p>Você possui {{ getTarefaspendentes().length }} tarefas pendentes</p>
    </header>


    <form  @submit.prevent="cadastratarefa()">
      <div class="row">
      <div class="col">
        <input @value="estado.tarefatemp" @change="evento => estado.tarefatemp = evento.target.value" required type="text" placeholder="Digite aqui a descrição da tarefa" class="form-control">
      </div>
      <div class="col-md-1">
        <button type="submit" class="btn btn-primary">Cadastrar</button>
      </div>
      <div class="col-md-2">
        <select  @change="evento => estado.filtro = evento.target.value" class="form-control">
          <option value="todas">Todas tarefas</option>
          <option value="pendentes">Pendentes</option>
          <option value="finalizadas">Finalizadas</option>
        </select>
      </div>
    </div>
    </form>
    


    <ul class="list-group mt-4">
      <li class="list-group-item" v-for="tarefa in getTarefasFiltradas()">
        <input @change="evento => tarefa.finalizada = evento.target.checked" :checked="tarefa.finalizada" id="tarefa.titulo" type="checkbox">
        <label :class="{done: tarefa.finalizada}" class="ms-3" for="tarefa.titulo">
          {{ tarefa.titulo }}</label>
      </li>
    </ul>

    </div>
</template>

<style scoped>

  .done{
    text-decoration: line-through;
  }

</style>
