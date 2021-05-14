<template>
  <div>
    <div class="card">
      <div class="card-image">
        <figure>
          <img :src="currentImg" alt="Placeholder image" />
        </figure>
      </div>
      <div class="card-content">
        <div class="media">
          <div class="media-left"></div>
          <div class="media-content">
            <p class="title is-4">{{ num }} - {{ name | upperLeter }}</p>
            <p class="subtitle is-6">{{ pokemon.type }}</p>
          </div>
        </div>
        <div class="content">
          <button @click="mudarSprite()" class="button is-medium">
            Mudar Sprite
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  created: async function () {
    try {
      // Quando nao colocamos as variaveis que utilizaremos no objeto
      // elas nao serao reativas, logo não irão ser carregadas
      // Precisamos instanciar o objeto e seu atributos 'pokemon'
      let response = await axios.get(this.url);
      this.pokemon.type = response.data.types[0].type.name;
      this.pokemon.front = response.data.sprites.front_default;
      this.pokemon.back = response.data.sprites.back_default;
      this.currentImg = this.pokemon.front;
    } catch (err) {
      console.log(err);
    }
  },
  data() {
    // Para a variavel ser reativa, deve ser instanciada aqui
    return {
      isFront: true,
      currentImg: "",
      pokemon: {
        type: "",
        front: "",
        back: "",
      },
    };
  },
  props: {
    num: Number,
    name: String,
    url: String,
  },
  filters: {
    upperLeter: function (value) {
      return value[0].toUpperCase() + value.slice(1);
    },
  },
  methods: {
    mudarSprite: function () {
      if (this.isFront) {
        this.isFront = false;
        this.currentImg = this.pokemon.back;
      } else {
        this.isFront = true;
        this.currentImg = this.pokemon.front;
      }
    },
  },
};
</script>

<style>
</style>