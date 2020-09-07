<template>
  <div>
    <div id="pokedex">
      <div id="leds">
        <div id="led"></div>
        <div id="led2"></div>
        <div id="led3"></div>
      </div>

      <h1>Pokedex</h1>
      <div class="logo"></div>

      <div id="preguntaPokemon">
        <input
          v-model="nombre"
          type="text"
          @keyup.enter="buscarPokemon"
          placeholder="Ingresa Nombre o Numero "
        />
        <button @click="buscarPokemon">cual es ese Pokemon?</button>
      </div>
<h2>Este es tu Pokemon:</h2>
        <h1>{{ nombrePokemon }}</h1>
      <div id="main">
        

        <div>
          <img id="img" :src="imagen" alt />
          <img id="img2" :src="imagen2" alt />
        </div>
      </div>
 <h1>Ataques</h1>
      <div id="ataques">
       
        <div v-for="(movimiento, i) in movimientos" :key="i">
          {{ movimiento.move.name }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "app",
  data() {
    return {
      nombre: "",
      pokemon: {
        name: "",
        sprites: {
          front_default: "",
          back_default: "",
        },
        moves: [],
      },
    };
  },
  created() {
    this.buscarPokemon();
  },
  methods: {
    async buscarPokemon() {
      await axios
        .get(this.url)
        .then((response) => (this.pokemon = response.data))
        .catch((error) => console.error("Fallo en la Url", error));
    },
  },
  computed: {
    imagen() {
      return this.pokemon.sprites.front_default;
    },
    imagen2() {
      return this.pokemon.sprites.back_default;
    },
    nombrePokemon() {
      return this.pokemon.name;
    },
    movimientos() {
      return this.pokemon.moves;
    },
    url() {
      return this.nombre == ""
        ? `${this.setUrl}pikachu`
        : `${this.setUrl}${this.nombre.toLowerCase()}`;
    },
    setUrl() {
      return "https://pokeapi.co/api/v2/pokemon/";
    },
  },
};
</script>

<style lang="scss" scoped>
#main {
  display: flex;
  justify-content: center;
  padding-top: 10px;
  background: white;
  border: 1px solid black;
  border-radius: 25px;
  margin: 10px 10px;
}

hr {
  border: 1px solid black;
}

#img,
#img2 {
  height: 200px;
}

p {
  font-size: 20px;
  text-align: center;
  font-weight: bold;
}

#pokedex {
  width: 1000px;
  background: red;
  border-radius: 25px;
  border: 3px solid darkblue;
  margin: auto;
}
h1 ,h2{
  text-align: center;
}

#leds {
  display: flex;
  margin-left: 25px;
  margin-top: 5px;
}

#led {
  width: 20px;
  height: 20px;
  background-color: #16c616;
  position: relative;
  border-radius: 25px;
  border: 1px solid black;
  transition: all 2s;
}
#led2 {
  width: 20px;
  height: 20px;
  background-color: yellow;
  position: relative;
  border-radius: 25px;
  border: 1px solid black;
  transition: all 2s;
}
#led3 {
  width: 20px;
  height: 20px;
  background-color: red;
  position: relative;
  border-radius: 25px;
  border: 1px solid black;
  transition: all 2s;
}

#preguntaPokemon {
  display: flex;
  justify-content: space-evenly;
  padding-top: 5px;
}

#ataques {
  background: white;
  border: 1px black solid;
  border-radius: 25px;
  display: flex;
  flex-wrap: wrap;
  text-align: center;
  padding-top: 10px;
  margin: 10px 10px;

  div {width: 50%;}

}
</style>
