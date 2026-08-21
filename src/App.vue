<script>
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

export default {
  components: {
    Sidebar,
    PhPlus,
    PhPencil,
    PhTrash,
    PhListChecks,
    PhCheck,
  },

  data() {
    return {
      categoriaSelezionata: "Oggi",
      attivitaGiornaliere: datiSalvati ? JSON.parse(datiSalvati) : [],
      divAggiungiNuovaAttivita: false,
      nuovaAttivita: "",
      idAttivitaDaModificare: null,
    };
  },

  computed: {
    attivitaCompletate() {
      return this.attivitaGiornaliere.filter(
        (attivita) =>
          attivita.completata === true &&
          (attivita.categoria ?? "Oggi") === this.categoriaSelezionata,
      );
    },

    attivitaDaCompletare() {
      return this.attivitaGiornaliere.filter(
        (attivita) =>
          attivita.completata === false &&
          (attivita.categoria ?? "Oggi") === this.categoriaSelezionata,
      );
    },
  },

  watch: {
    attivitaGiornaliere: {
      handler(nuovoValore) {
        localStorage.setItem(STORAGE_KEY, JSON.stringify(nuovoValore));
      },
      deep: true,
    },
  },

  methods: {
    cambiaCategoria(categoria) {
      this.categoriaSelezionata = categoria;
    },

    chiudiNuovaAttivita() {
      this.divAggiungiNuovaAttivita = false;
    },

    aggiungiNuovaAttivita() {
      if (this.nuovaAttivita.trim() === "") {
        return;
      }

      this.attivitaGiornaliere.push({
        id: Date.now(),
        todo: this.nuovaAttivita.trim(),
        completata: false,
        categoria: this.categoriaSelezionata,
      });

      this.nuovaAttivita = "";
      this.divAggiungiNuovaAttivita = false;
    },

    eliminaAttivita(idDaEliminare) {
      this.attivitaGiornaliere = this.attivitaGiornaliere.filter(
        (attivita) => attivita.id !== idDaEliminare,
      );
    },

    modificaAttivita(idDaModificare) {
      this.idAttivitaDaModificare = idDaModificare;
    },

    confermaModifica() {
      this.idAttivitaDaModificare = null;
    },
  },
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

            <span v-if="idAttivitaDaModificare !== attivita.id">
              {{ attivita.todo }}
            </span>
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
