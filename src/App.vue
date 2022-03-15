<template>
<div>
    <h1>PokeGuía</h1>
    <div class="formulario">
      <label for="">Nombre: </label>
      <input @keydown.enter="cargar" v-model="name" type="text" />
      <button @click="cargar">Buscar</button>
    </div>
    <div v-if="condicion" class="poke-data">
      <img :src="pokefoto" />
      <h2>Movimientos</h2>
      <ul>
        <li v-for="(movimiento, index) in movimietosPokemon" :key="index">
          {{ movimiento.move.name }}
        </li>
      </ul>

      <h2>Habilidades</h2>
      <ul>
        <li v-for="(habilidad, index) in habilidadesPokemon" :key="index">
          {{ habilidad.ability.name }}
        </li>
      </ul>
    </div>
    <p class="error" v-else>Pokemon no encontrado...</p>
    </div>
</template>

<script>

export default {
  name: 'App',
  components: {
    
  },
  data() {
    return {
      name: "pikachu",
      pokedata: {},
      condicion: true,
    };
  },
    created() {
    this.cargar();
  },
  methods: {
       cargar() {
      fetch(`https://pokeapi.co/api/v2/pokemon/${this.name}`)
        .then((response) => response.json())
        .then((data) => {
          this.condicion = true;
          this.pokedata = data;
        }).catch((error) => {
          console.log(error);
          this.condicion = false;
        });
    },
  },
  computed:{
        pokefoto() {

        if (!Object.prototype.hasOwnProperty.call(this.pokedata,"sprites")) {
        return;
      }

      return this.pokedata.sprites.front_shiny;
    },
        movimietosPokemon() {
              if (!Object.prototype.hasOwnProperty.call(this.pokedata,"moves")) {
        return;
      }

      let five = this.pokedata.moves.slice(0, 4);
      return five;
    },
        habilidadesPokemon() {
      return this.pokedata.abilities;
    },

  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
