<script setup>
import { ref, computed, watch } from "vue";
import {
  PhPlus,
  PhPencil,
  PhTrash,
  PhListChecks,
  PhCheck,
} from "@phosphor-icons/vue";
import Sidebar from "./components/Sidebar.vue";

const STORAGE_KEY = "attivita-giornaliere";

const datiSalvati = localStorage.getItem(STORAGE_KEY);

const categoriaSelezionata = ref("Oggi");

const cambiaCategoria = (categoria) => {
  categoriaSelezionata.value = categoria;
};

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
  return attivitaGiornaliere.value.filter(
    (attivita) => 
    attivita.completata === true &&
      (attivita.categoria ?? "Oggi") === categoriaSelezionata.value,
  );
});

const attivitaDaCompletare = computed(() => {
  return attivitaGiornaliere.value.filter(
    (attivita) => attivita.completata === false &&
      (attivita.categoria ?? "Oggi") === categoriaSelezionata.value,
  );
});

const aggiungiNuovaAttivita = () => {
  if (nuovaAttivita.value.trim() === "") {
    return;
  }

  attivitaGiornaliere.value.push({
    id: Date.now(),
    todo: nuovaAttivita.value.trim(),
    completata: false,
    categoria: categoriaSelezionata.value,
  });

  nuovaAttivita.value = "";
  divAggiungiNuovaAttivita.value = false;
};

const eliminaAttivita = (idDaEliminare) => {
  attivitaGiornaliere.value = attivitaGiornaliere.value.filter(
    (attivita) => attivita.id !== idDaEliminare,
  );
};

const idAttivitaDaModificare = ref(null);

const modificaAttivita = (idDaModificare) => {
  idAttivitaDaModificare.value = idDaModificare;
};

const confermaModifica = () => {
  idAttivitaDaModificare.value = null;
};
</script>

<template>
  <div class="layout">
    <div class="sidebar">
      <Sidebar @seleziona-categoria="cambiaCategoria" />
    </div>
    <div class="contenuto">
      <header class="header">
        <div class="titolo-con-icona">
          <PhListChecks :size="32" />
          <h1>{{ categoriaSelezionata }}</h1>
        </div>
      </header>

      <main class="contenitore" @click="chiudiNuovaAttivita">
        <div class="card">
          <header class="header-card">
            <h2>Da Completare</h2>

            <PhPlus :size="22" @click.stop="divAggiungiNuovaAttivita = true" />
          </header>

          <div
            v-if="divAggiungiNuovaAttivita"
            class="elemento-lista"
            @click.stop
          >
            <input
              type="text"
              v-model="nuovaAttivita"
              placeholder="Scrivi una nuova attività"
            />

            <button type="button" @click="aggiungiNuovaAttivita">
              Conferma
            </button>
          </div>

          <div
            class="elemento-lista"
            v-for="attivita in attivitaDaCompletare"
            :key="attivita.id"
          >
            <input type="checkbox" v-model="attivita.completata" />

            <span v-if="idAttivitaDaModificare !== attivita.id">{{
              attivita.todo
            }}</span>
            <input v-else v-model="attivita.todo" type="text" />

            <PhPencil
              :size="22"
              v-if="idAttivitaDaModificare !== attivita.id"
              @click="modificaAttivita(attivita.id)"
            />
            <button v-else type="button" @click="confermaModifica">
              Conferma
            </button>

            <PhTrash :size="22" @click="eliminaAttivita(attivita.id)" />
          </div>
        </div>

        <div class="card">
          <header class="header-card">
            <h2>Completate</h2>
            <PhCheck :size="22" />
          </header>
          <div
            class="elemento-lista"
            v-for="attivita in attivitaCompletate"
            :key="attivita.id"
          >
            <input type="checkbox" v-model="attivita.completata" />

            <span class="elemento-lista-completate">{{ attivita.todo }}</span>
          </div>
        </div>
      </main>
    </div>
  </div>
</template>

