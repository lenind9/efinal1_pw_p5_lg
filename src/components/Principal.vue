<template>
  <div class="container">
    <div>
      <div v-show="victoria()" class="win">
        <p>Puntaje: {{ puntaje }}</p>
        <p>Felicitaciones has ganado un premio de $10.000,00</p>
        <button @click="reiniciar">Nuevo juego</button>
      </div>
      <div v-show="perdida()" class="lose">
        <p>Has utilizado tus 5 intentos</p>
        <p>El juego ha terminado, intentalo nuevamente</p>
        <button @click="reiniciar">Nuevo juego</button>
      </div>

      <div v-if="!perdida() && !victoria()">
        <div class="score">
          <label for="">Puntaje: {{ puntaje }} </label>
          <label for="">Intentos: {{ intentos }}</label>
        </div>
        <div class="comp">
          <PokemonComp :pokemon="pokemons[0]" />
          <PokemonComp :pokemon="pokemons[1]" />
          <PokemonComp :pokemon="pokemons[2]" />
        </div>
      </div>
      <button v-if="!perdida() && !victoria()" @click="jugar">Jugar</button>
    </div>
  </div>
</template>

<script>
import PokemonComp from "./PokemonComp.vue";

export default {
  components: {
    PokemonComp,
  },
  data() {
    return {
      pokemons: [
        {
          imagen:
            "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/1.svg",
          respuesta: "",
        },
        {
          imagen:
            "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/2.svg",
          respuesta: "",
        },
        {
          imagen:
            "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/3.svg",
          respuesta: "",
        },
      ],
      puntaje: 0,
      intentos: 0,
    };
  },
  methods: {
    async jugar() {
      await this.obtenerRespuestas();
      this.calcularPuntaje();
    },
    async obtenerRespuestas() {
      for (let i = 0; i < 3; i++) {
        const respuesta = await this.consumirAPI();
        this.pokemons[i].respuesta = respuesta.answer;
        this.pokemons[i].imagen = respuesta.image;
      }
    },
    async consumirAPI() {
      const respuesta = await fetch("https://yesno.wtf/api").then((res) =>
        res.json()
      );
      return respuesta;
    },
    calcularPuntaje() {
      const respUnica = [];
      for (const pokemon of this.pokemons) {
        if (!respUnica.includes(pokemon.respuesta)) {
          respUnica.push(pokemon.respuesta);
        }
      }
      this.puntaje +=
        respUnica.length === 1 ? 5 : respUnica.length === 2 ? 2 : 0;
      this.intentos++;
    },
    perdida() {
      return this.puntaje < 10 && this.intentos === 5;
    },
    victoria() {
      return this.puntaje >= 10;
    },
    reiniciar() {
      this.pokemons = [
        {
          imagen:
            "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/1.svg",
          respuesta: "",
        },
        {
          imagen:
            "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/2.svg",
          respuesta: "",
        },
        {
          imagen:
            "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/3.svg",
          respuesta: "",
        },
      ];
      this.puntaje = 0;
      this.intentos = 0;
    },
  },
};
</script>

<style>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
}

.win,
.lose {
  display: grid;
  border: 2px solid black;
  padding: 25px;
  font-size: 20px;
}

.win {
  color: blue;
}

.lose {
  color: red;
}

.score {
  display: grid;
  grid-template-columns: repeat(2, 250px);
  font-weight: bold;
  margin-left: 20%;
  margin-bottom: 10px;
  
}

.comp {
  display: grid;
  grid-template-columns: repeat(3, 250px);
  margin-bottom: 25px;
}

button {
  width: 150px;
  height: 30px;
  margin: 0 auto;
  display: block
}

</style>
