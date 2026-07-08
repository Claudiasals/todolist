<script setup>

import { ref, computed } from "vue"

const attivitagiornaliere = ref ([ 
  {id: 1, todo: "Fare la spesa", completata: false },
  {id: 2, todo: "Lavare il cane", completata: false },
  {id: 3, todo: "Spostare appuntamento", completata: false }
]);

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
    </div>

    <span> </span>


    <h2>Completate</h2>
     <div v-for= "attivita in attivitacompletate" :key="attivita.id">
   


      <span>{{ attivita.todo }}</span> 
    </div>
    
  </main>
</template>

