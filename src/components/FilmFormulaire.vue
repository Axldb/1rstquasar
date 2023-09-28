<template>
    <div>
        <label>Title</label>
        <input v-model="film.titre" name="title" id="titre" type="text" />
        <label>Date</label>
        <input v-model="film.date" date="date" id="date" type="number" />
        <br />
        <br />
        <select v-model="film.idCatFilm" id="category">
            <option value="1">Comedie</option>
            <option value="2">SF</option>
            <option value="3">Action</option>
            <option value="4">Horreur</option>
        </select>

        <br />
        <br />
        <button v-show = "isEditing" @click="Updatebutton">Update</button>
        <button v-show="!isEditing" @click="handleSubmit">Create</button>



    </div>

</template>
<script setup lang="ts">

    import { ref, onMounted, defineEmits,defineProps,watch } from 'vue';
    import axios from 'axios';
    import { Film } from '../FilmModele/Film.ts';


    const isEditing = ref(false);
    const emit = defineEmits(['update','reset']);
    
    const props = defineProps<{
        tmpfilm: Film
    }>();
    
    const film = ref<Film>({

        idCatFilm: 1
    } as Film);
    
    
 

    
    const handleSubmit = async () => {
        try {
            const response = await axios.post('https://localhost:7195/Film/Add', film.value);

            emit('update','reset');
  
        } catch (error) {
            console.error(error);
        }

        

    };
    async function Updatebutton() {
        try {
            const response = await axios.put(`https://localhost:7195/Film/Update/${film.value.id}`, film.value);

            console.log('Mise à jour réussie :', response.data);
            emit('update');
            emit('reset')
           
        } catch (error) {
            console.error('Erreur lors de la mise à jour du film :', error);
        }
    }

    watch(() => props.tmpfilm, (newtmpfilm) => {
    if (newtmpfilm && newtmpfilm.length > 0) {
        
        film.value = newtmpfilm[0] as Film;
        isEditing.value = true;
    }else{emit('reset')}
});

    //watch(chosequivachanger, functionaexecuter)



    onMounted(() => {
        if (props.tmpfilm && props.tmpfilm.length > 0) {
            film.value = props.tmpfilm[0] as Film;
            isEditing.value = true;
        }
    })
    
</script>