<script>
import { PhPlus, PhTornado } from "@phosphor-icons/vue";

export default {
    components: {
        PhPlus,
        PhTornado
    },

    emits: ["selezionaCategoria"], 

    data() {
        return {
            categorie: ["Oggi", "Lavoro", "Shopping", "Viaggi"]
        }
    },

    methods: {
        selezionaCategoria(categoria) {
            this.$emit("selezionaCategoria", categoria)
        }    }
}


</script>



<template>
  <aside class="sidebar-contenuto">
    <div class="titolo-con-icona">
      <PhTornado :size="32" />
      <p>Le mie liste</p>
    </div>
    <nav class="menu-liste">
      <button
        class="voce-lista"
        v-for="categoria in categorie"
        :key="categoria"
        @click="selezionaCategoria(categoria)"
      >
        {{ categoria }}
      </button>

      <button class="nuova-lista">
        <PhPlus :size="22" />
        Nuova lista
      </button>
    </nav>
  </aside>
</template>

<style scoped>
.sidebar-contenuto {
  height: 100%;
  padding: 32px 22px;
  background-color: white;
  border-radius: 28px;
  box-shadow: 0 10px 35px rgba(30, 35, 70, 0.08);
}

.menu-liste {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.voce-lista,
.nuova-lista {
  width: 100%;
  padding: 14px 16px;
  border: none;
  border-radius: 14px;
  background-color: transparent;
  font-size: 16px;
  text-align: left;
  cursor: pointer;
}

.voce-lista:hover {
  background-color: rgb(244, 241, 255);
  color: rgb(99, 72, 235);
}

.nuova-lista {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-top: 20px;
  color: rgb(99, 72, 235);
}

.nuova-lista:hover {
  background-color: rgb(244, 241, 255);
}
</style>
