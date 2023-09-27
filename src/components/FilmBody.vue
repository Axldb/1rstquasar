<template>

    <div class="q-pa-md">
      <q-table
        flat bordered
        title="Films"
        :rows="films"
        :columns="columns"
        row-key="id"
        selection="single"
        v-model:selected="selectedFilm.value"
      />
    </div>
    <div class="q-mt-md">
      {{selectedFilm.value}}
    </div>
    <div class="btn-group btn-group-lg" role="group" aria-label="Basic example">
        <button class="btn btn-secondary btn-lg" id="button" @click="showForm" v-bind:disabled="selectedFilm !== {}">Add</button>

        <button class="btn btn-danger btn-lg" id="button" @click="deleteSelectedFilms" v-bind:disabled="selectedFilm.titre === null">Delete</button>
    </div>
    <br />
    <br />
    <FilmFormulaire :tmpfilm="selectedFilm" @update="fetchFilms();" @closeForm="handleshowForm" v-if="isVisible" />
    <br />
    <br />
  </template>
  
  <script setup lang="ts">
  import { ref, onMounted, } from 'vue'
  import axios from 'axios'
  import { Film } from '../FilmModele/IFilm.ts'



  let selectedFilm = ref<Film>({} as Film);

  
    if (selectedFilm.value.length > 0) {
      selectedFilm.value = selectedFilm.value[0];
    }
  
  // selectedFilm=selectedFilm.value[0]
  const columns = [
    { name: 'titre', align: 'left', label: 'Titre', field: 'titre', sortable: true },
    { name: 'date', align: 'center', label: 'Date', field: 'date', sortable: true },
    { name: 'categorie', label: 'Catégorie', field: 'categorie', sortable: true }
  ]
  const films = ref<Film[]>([])
  
  function handleshowForm() {
        selectedFilm.value = null;
        showForm()
    }
  const isVisible = ref(false);
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

    const filmId = selectedFilm.value.id;
    await deleteFilmById(filmId);
    }
async function deleteFilmById(filmId: number) {
        try {
            await axios.delete(`https://localhost:7195/Film/Del?id=${filmId}`);
            // Actualisez la liste des films après la suppression
            fetchFilms();
        } catch (error) {
            console.error(`Erreur lors de la suppression du film avec l'ID ${filmId}:`, error);
        }
    }
  
  onMounted(() => {
    fetchFilms() // Appeler la fonction pour récupérer les données lors de l'initialisation
  })
  </script>
  