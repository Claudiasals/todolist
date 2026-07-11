<script setup>
import { ref, computed, watch } from "vue";
import { PhPlusCircle, PhPencil, PhTrash } from "@phosphor-icons/vue";



const STORAGE_KEY = "attivita-giornaliere";

const datiSalvati = localStorage.getItem(STORAGE_KEY);

const attivitagiornaliere = ref(datiSalvati ? JSON.parse(datiSalvati) : []);

const divAggiungiNuovaAttivita = ref(false);

const nuovaAttivita = ref("");



watch(
  attivitagiornaliere,
  (nuovoValore) => {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(nuovoValore));
  },
  { deep: true },
);

const attivitacompletate = computed(() => {
  return attivitagiornaliere.value.filter((attivita) => attivita.completata);
});

const attivitadacompletare = computed(() => {
  return attivitagiornaliere.value.filter(
    (attivita) => attivita.completata === false,
  );
});

const aggiungiNuovaAttivita = () => {
  if (nuovaAttivita.value.trim() === "") {
    return;
  } 
  attivitagiornaliere.value.push(
{
  id: Date.now(),
  todo: nuovaAttivita.value.trim(),
  completata: false
}
);
  nuovaAttivita.value = "";
  divAggiungiNuovaAttivita.value = false;
};


</script>

<template>
  <header class="header">
    <h1>Attività giornaliere</h1>
  </header>

  <main class="contenitore">
    <div class="card">
      <header class="header-card">
        <h2>Da fare</h2>

        <PhPlusCircle :size="32" @click="divAggiungiNuovaAttivita = true" />
      </header>

      <div  v-if="divAggiungiNuovaAttivita" class="elemento-lista">
        <input type="text" v-model="nuovaAttivita" placeholder="Scrivi una nuova attività" />

        <button type="button" @click="aggiungiNuovaAttivita">Conferma</button>
      </div>

      <div
        class="elemento-lista"
        v-for="attivita in attivitadacompletare"
        :key="attivita.id"
      > 
        <input type="checkbox" v-model="attivita.completata" />

        <span>{{ attivita.todo }}</span>
        <!-- {{...}} interpolazione Mustache: x stampare un dato dentro i ltemplate-->

        <PhPencil :size="32" />
        <PhTrash :size="32" />
      </div>
    </div>

    <div class="card">
      <header class="header-card">
        <h2>Completate</h2>
      </header>
      <div
        class="elemento-lista"
        v-for="attivita in attivitacompletate"
        :key="attivita.id"
      >
        <input type="checkbox" v-model="attivita.completata" />

        <span>{{ attivita.todo }}</span>
      </div>
    </div>
  </main>
</template>
