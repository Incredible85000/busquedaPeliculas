<script>
import { ref } from "vue";

import { eventBus } from "../eventBus";

export default {
  data() {
    return {
      palabra: "",
    };
  },
  methods: {
    accion(pala) {
      eventBus.emit("dataUpdated", pala);
    },
  },
};

/* defineProps({
  msg: String,
});

const count = ref(1);
const animal = ref("Perro"); */
</script>

<template>
  <center><h2>Lista de selección de películas</h2></center>
  <div class="search-form">
    <input
      type="text"
      placeholder="Ejemplo, lo que el viento se llevo..."
      class="search-input"
      name="busque"
      id="busque"
      v-model="palabra"
      v-on:keyup.enter="accion(palabra)"
    />
    <button type="submit" class="search-button" v-on:click="accion(palabra)">
      Buscar
    </button>
  </div>
  <!-- <h1>{{ msg }}</h1>

  <div class="card">
    <button type="button" @click="count++">count is {{ count }}</button>
    <button type="button" @click="animal = 'Gato'">
      Haga click para cambiar el animal
    </button>
    <span>{{ animal }}</span>
  </div> -->
</template>

<style scoped>
.search-form {
  display: flex;
  justify-content: center;
  margin-top: 50px;
}

.search-input:focus {
  border-color: greenyellow !important;
}

.search-input {
  width: 300px;
  height: 30px;
  padding: 5px;
}

.search-button {
  height: 30px;
  margin-left: 10px;
}

@media (max-width: 600px) {
  .search-input {
    width: 200px;
  }
}

h2 {
  font-family: Gill Sans, Verdana;
  font-size: 11px;
  line-height: 14px;
  text-transform: uppercase;
  letter-spacing: 2px;
  font-weight: bold;
  margin: 50px;
}
</style>
