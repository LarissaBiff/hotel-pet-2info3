
<script setup>
import { onMounted,ref } from 'vue'; 

const API_URL = 'http://localhost:3000';

const pets = ref([]);
const tutores = ref([]);

async function carregarDados() {

  const respostaPets = await fetch(`${API_URL}/pets`);
  pets.value = respostaPets.json();
  
  const respostaTutores = await fetch(`${API_URL}/tutores`);
  pets.value = respostaTutores.json();
}
</script>

<template>
  <table>
    <thead>
      <th>
        Espécie
      </th>
    </thead>
  </table>
  <table>
    <tbody 
    v-for="pet in pets"
    :key="pets.id">
      <td>

      </td>
    </tbody>
  </table>
  <div>
    <header class="mb-4">
      <h1 class="text-2xl font-bold">Listagem de Pets</h1>
      <p class="text-body-secondary mb-0">
        Listagem dos Pets cadastrados no sistema.
      </p>
    </header>

    <RouterLink
      class="btn btn-primary"
      :to="{ name: 'addPet' }"
    >
      Adicionar Pet
    </RouterLink>
  </div>
</template>