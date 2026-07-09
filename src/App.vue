<script setup>

import { ref, computed, watch } from "vue"

const STORAGE_KEY = "attivita-giornaliere"

const datiSalvati = localStorage.getItem(STORAGE_KEY)

const attivitagiornaliere = ref (
  datiSalvati
    ? JSON.parse(datiSalvati)
  : [ 
  {id: 1, todo: "Fare la spesa", completata: false },
  {id: 2, todo: "Lavare il cane", completata: false },
  {id: 3, todo: "Spostare appuntamento", completata: false }
]);

watch (attivitagiornaliere,
  (nuovoValore) => { localStorage.setItem(STORAGE_KEY, JSON.stringify(nuovoValore)) },
  { deep: true }
)


const attivitacompletate = computed(() => { 
  return attivitagiornaliere.value.filter(attivita => attivita.completata);
});

const attivitadacompletare = computed(() => { 
  return attivitagiornaliere.value.filter(attivita => attivita.completata === false);
});

</script>

<template>
  <main>
    <h1>Attività giornaliere</h1>
    <h2>Da fare</h2>

    <div v-for= "attivita in attivitadacompletare" :key="attivita.id">
     

      <input type="checkbox" v-model="attivita.completata" />
  
      <span>{{ attivita.todo }}</span> 
    <!-- {{...}} interpolazione Mustache: x stampare un dato dentro i ltemplate-->
    </div>

    <span> </span>


    <h2>Completate</h2>
     <div v-for= "attivita in attivitacompletate" :key="attivita.id">


      <span>{{ attivita.todo }}</span> 
    </div>
    
  </main>
</template>

