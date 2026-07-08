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
     <!-- v-for= "nome oggetto in nome array" per ciclare gli elementi dell'array. 
      per ogni oggetto identificato da id crea un div -->

      <input type="checkbox" v-model="attivita.completata" />
      <!-- v-model legge il valore del dato e
      se cambia il checkbox cambia anche il valore del dato "completata" e viceversa -->

      <span>{{ attivita.todo }}</span> 
    </div>

    <span> </span>


    <h2>Completate</h2>
     <div v-for= "attivita in attivitacompletate" :key="attivita.id">
   

  

      <span>{{ attivita.todo }}</span> 
    </div>
    
  </main>
</template>


<!-- 

- computed è una funzione che crea un oggetto reattivo che dipende da altri oggetti reattivi.
  Serve a creare un dato calcolato automaticamente partendo da un altro dato. 

- ref è una funzione che crea un oggetto reattivo 

-->