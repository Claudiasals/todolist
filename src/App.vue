<script setup>
import { ref, computed, watch, nextTick } from "vue";
import { PhPlusCircle, PhPencil, PhTrash } from "@phosphor-icons/vue";



const STORAGE_KEY = "attivita-giornaliere";

const datiSalvati = localStorage.getItem(STORAGE_KEY);

const attivitaGiornaliere = ref(datiSalvati ? JSON.parse(datiSalvati) : []);

const chiudiNuovaAttivita = () => {
  divAggiungiNuovaAttivita.value = false;
};

const divAggiungiNuovaAttivita = ref(false);

const nuovaAttivita = ref("");

watch(
  attivitaGiornaliere,
  (nuovoValore) => {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(nuovoValore));
  },
  { deep: true },
);

const attivitaCompletate = computed(() => {
  return attivitaGiornaliere.value.filter((attivita) => attivita.completata);
});

const attivitaDaCompletare = computed(() => {
  return attivitaGiornaliere.value.filter(
    (attivita) => attivita.completata === false,
  );
});

const aggiungiNuovaAttivita = () => {
  if (nuovaAttivita.value.trim() === "") {
    return;
  } 
  attivitaGiornaliere.value.push(
{
  id: Date.now(),
  todo: nuovaAttivita.value.trim(),
  completata: false
}
);
  nuovaAttivita.value = "";
  divAggiungiNuovaAttivita.value = false;
};

const eliminaAttivita = (idDaEliminare) => {
attivitaGiornaliere.value = attivitaGiornaliere.value.filter(
  (attivita) => attivita.id !== idDaEliminare
);
} 

const idAttivitaDaModificare = ref(null);

const modificaAttivita = (idDaModificare) => {
  idAttivitaDaModificare.value = idDaModificare;

}

const confermaModifica = () => {
idAttivitaDaModificare.value = null;

}

</script>

<template>
  <header class="header">
    <h1>Attività giornaliere</h1>
  </header>

  <main class="contenitore"  @click="chiudiNuovaAttivita">
    <div class="card">
      <header class="header-card">
        <h2>Da fare</h2>

        <PhPlusCircle :size="32" @click.stop="divAggiungiNuovaAttivita = true" />
      </header>

      <div  v-if="divAggiungiNuovaAttivita" class="elemento-lista"   @click.stop>
        <input type="text" v-model="nuovaAttivita" placeholder="Scrivi una nuova attività" />

        <button type="button" @click="aggiungiNuovaAttivita">Conferma</button>
      </div>

      <div
        class="elemento-lista"
        v-for="attivita in attivitaDaCompletare"
        :key="attivita.id"
      > 
        <input type="checkbox" v-model="attivita.completata" />

        <span v-if="idAttivitaDaModificare  !== attivita.id">{{ attivita.todo }}</span>
        <input v-else v-model="attivita.todo" type="text" />
         

        <PhPencil :size="22" v-if="idAttivitaDaModificare  !== attivita.id" @click="modificaAttivita(attivita.id)"/>
        <button v-else type="button" @click="confermaModifica">Conferma</button>

        <PhTrash :size="22"  @click="eliminaAttivita(attivita.id)"/>
      </div>
    </div>

    <div class="card">
      <header class="header-card">
        <h2>Completate</h2>
      </header>
      <div
        class="elemento-lista "
        v-for="attivita in attivitaCompletate"
        :key="attivita.id"
      >
        <input type="checkbox" v-model="attivita.completata" />

        <span class="elemento-lista-completate">{{ attivita.todo }}</span>
      </div>
    </div>
  </main>
</template>
