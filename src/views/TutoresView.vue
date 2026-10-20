<script setup>
import { onMounted, ref } from 'vue';

const API_URL = 'http://localhost:3000';
const tutores = ref([]);
const pets = ref([]);

async function carregarDados() {
  const respostaTutores = await fetch(`${API_URL}/tutores`);

  if (!respostaTutores.ok) {
    console.error('Erro ao carregar tutores');
    return;
  }

  const respostaQuantoPet = await fetch(`${API_URL}/pets`);

  if (!respostaQuantoPet.ok) {
    console.error('Erro ao carregar pets');
    return;
  }

  pets.value = await respostaQuantoPet.json();

  tutores.value = await respostaTutores.json();
}

function listaPetsTutor(id_tutor){

return pets.value.filter((pet) => pet.tutorId === id_tutor)

}

function nomesPetsTutor(idTutor){ 
  const nomes = listaPetsTutor(idTutor); 
  const listaNomes = nomes.map((pet) => pet.nome);
  return listaNomes.join(' / ');
}


async function deletarTutor(id) {
  const confirmarDelete = window.confirm('Deseja realmente deletar esse perfil de tutor?');

  if (!confirmarDelete) return;

  const resposta = await fetch(`${API_URL}/tutores/${id}`, {
    method: 'DELETE',
  });

  if (!resposta.ok) {
    console.error('Erro ao excluir tutor');
    return;
  }

  await carregarDados();
}

onMounted(carregarDados);
</script>

<template>
  <div>
    <header class="mb-4">
      <h1 class="text-2xl font-bold">Listagem de Tutores</h1>
      <p class="text-body-secondary mb-0">
        Listagem dos tutores cadastrados no sistema.
      </p>
    </header>
  </div>

  <div>
    <router-link to="/tutores/novo">+ Cadastrar Tutor</router-link>
  </div>

  <table class="table table-striped table-hover">
    <thead>
      <tr>
        <th>ID</th>
        <th>Nome</th>
        <th>Telefone</th>
        <th>Pets (quantos)</th>
        <th>Pets (nome)</th>
        <th>Ações</th>
      </tr>
    </thead>
    <tbody>
      <tr
        v-for="tutor in tutores"
        :key="tutor.id"
      >
        <td>{{ tutor.id }}</td>
        <td>{{ tutor.nome }}</td>
        <td>{{ tutor.telefone }}</td>
        <td>{{ listaPetsTutor(tutor.id).length }}</td>
        <td>{{ nomesPetsTutor(tutor.id) }}</td>
        <td>
          <RouterLink
            class="btn btn-primary me-2"
            :to="`/tutores/${tutor.id}`"
          >
            Visualizar
          </RouterLink>
          <button
            @click="deletarTutor(tutor.id)"
            class="btn btn-danger"
          >
            Deletar
          </button>
        </td>
      </tr>
    </tbody>
  </table>
</template>
