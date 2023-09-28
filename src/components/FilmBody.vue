<template>

    <div class="q-pa-md">
      <q-table
        flat bordered
        title="Films"
        :rows="films"
        :columns="columns"
        row-key="id"
        selection="single"
       v-model:selected="selectedFilm"
      />
    </div>

    <div class="btn-group btn-group-lg" role="group" aria-label="Basic example">
        <button class="btn btn-secondary btn-lg" id="button" @click="showForm" v-bind:disabled="isAddDisabled">Add</button>

        <button class="btn btn-danger btn-lg" id="button" @click="deleteSelectedFilms" v-bind:disabled="isDisabled">Delete</button>
    </div>
    <br />
    <br />
    <FilmFormulaire :tmpfilm="selectedFilm" @update="fetchFilms();" @reset="selectedFilmReset"  v-if="selectedFilm && Object.keys(selectedFilm).length > 0 || isVisible " />
    <br />
    <br />
  </template>
  
  <script setup lang="ts">
  import { ref, onMounted,watch } from 'vue'
  import axios from 'axios'
  import { Film } from '../FilmModele/Film.ts'
  import FilmFormulaire from './FilmFormulaire.vue';
  
  const isDisabled = ref(true);
  const isAddDisabled = ref(false);
  const selectedFilm = ref<Film>();
    const isVisible = ref(false);

  

watch(selectedFilm,(NV :Film)=>{
  const test= selectedFilm.value === undefined ||selectedFilm.value.length < 1 
    isDisabled.value = test;
  isAddDisabled.value = !test;
})



  const columns = [
    { name: 'titre', align: 'left', label: 'Titre', field: 'titre', sortable: true },
    { name: 'date', align: 'center', label: 'Date', field: 'date', sortable: true },
    { name: 'categorie', label: 'Catégorie', field: 'categorie', sortable: true }
  ]
  const films = ref<Film[]>([])
  
  function selectedFilmReset(){
    selectedFilm.value = undefined;
  }


    const showForm = () => {
        isVisible.value = !isVisible.value;
    };
  async function fetchFilms() {
    try {
      const response = await axios.get('https://localhost:7195/Film/GetListFilm')
      films.value = response.data // Mettre à jour les films avec les données de l'API
    } catch (error) {
      console.error('Erreur lors de la récupération des films depuis l\'API:', error)
    }
  }
  async function deleteSelectedFilms() {

    const filmId = selectedFilm.value[0].id;
        try {
            await axios.delete(`https://localhost:7195/Film/Del?id=${filmId}`);
            // Actualisez la liste des films après la suppression
            fetchFilms();
            selectedFilmReset();
        } catch (error) {
            console.error(`Erreur lors de la suppression du film avec l'ID ${filmId}:`, error);
        }
    }
  
  onMounted(() => {
    fetchFilms()
  })
  </script>
