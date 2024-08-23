<script>
import { ref } from "vue";

import { eventBus } from "../eventBus";

export default {
  data() {
    return {
      palabra: "",
      infopeliculas: [],
      showModal: false,
      modalContent: {},
    };
  },
  mounted() {
    eventBus.on("dataUpdated", (data) => {
      this.palabra = data;
      this.basePeliculas();
    });
  },
  methods: {
    async basePeliculas() {
      const apiKey = "2f638e70d8c162ca10cd3962ba248895";
      const apiUrl = "https://api.themoviedb.org";
      const query = this.palabra;

      const url = `${apiUrl}/3/search/movie?query=${query}&api_key=${apiKey}`;

      console.log(url);

      const resultado = await fetch(url)
        .then((res) => res.json())
        .catch((err) => console.error(err));

      const { results } = resultado;

      console.log(results);

      this.infopeliculas = results;

      /* https://image.tmdb.org/t/p/w500/ */

      /* poster_path
: 
"/AfFD10ZqEx2vkxM2yvRZkybsGB7.jpg"
release_date
: 
"1998-12-31"
title
: 
"Matrix" */
      /* console.log(results[0].poster_path);
      console.log(results[0].release_date);
      console.log(results[0].title); */
    },
    openModal(infope) {
      this.modalContent = infope;
      this.showModal = true;
    },
    closeModal() {
      this.showModal = false;
      this.modalContent = {};
    },
  },
};
</script>

<template>
  <div class="contenedor">
    <div v-if="showModal">
      <div @click.stop>
        <span @click="closeModal"><button>Cerrar descripción</button></span>
        <div>
          <img
            :src="`https://image.tmdb.org/t/p/w300/${modalContent.poster_path}`"
            alt=""
          />
        </div>
        <h2>Title: {{ modalContent.title }}</h2>
        <h2>Overview: {{ modalContent.overview }}</h2>
        <h2>Release Date: {{ modalContent.release_date }}</h2>
        <h2>Vote average: {{ modalContent.vote_average }}</h2>
      </div>
    </div>

    <div
      class="item"
      v-bind:key="index"
      v-for="(infope, index) in infopeliculas"
    >
      <a href="#" @click.prevent="openModal(infope)">
        <img
          :src="`https://image.tmdb.org/t/p/w300/${infope.poster_path}`"
          alt=""
          :class="responsiveimg"
        />
        <div class="textoverlay">
          <p>Name: {{ infope.title }}</p>
          <p>Release Date: {{ infope.release_date }}</p>
        </div>
      </a>
    </div>
  </div>
</template>


<style>
.contenedor {
  position: relative;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  gap: 5px;
  padding: 20px;
  height: 100vh;
}

.item {
  height: 350px;
  width: 300px;
  background: #fb6942;
  border-radius: 1rem;
  color: #fff;
  position: relative;
  overflow: hidden;
}

.responsiveimg {
  max-width: 100%;
  max-height: 100%;
  object-fit: cover;
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

.textoverlay {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 10px;
  margin: 4px;
  background-color: rgba(0, 0, 0, 0.5);
  text-align: right;
  font-weight: bold;
  color: #fff;
  text-overflow: ellipsis;
  white-space: nowrap;
  overflow: hidden;
  z-index: 2;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 13px;
}

@media (max-width: 600px) {
  .item {
    width: 70vw;
    height: auto;
  }
}
</style>