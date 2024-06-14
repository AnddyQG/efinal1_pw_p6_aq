<template> 
<div class="general">
  <div class="container">
    <div v-if="intento < 5">
      <div class="image">
   
        <div class="valores">
          <h1>Puntaje :{{ puntaje }}</h1>
          <h1>Intento :{{ intento }}</h1>
        </div>
      </div>
      <div class="pantalla">
        <div>
          <img class="pokemon-img" :src="img1" alt="no hay imagen">
          <h1 class="texto1">{{ texto1 }}</h1>
        </div>
        <div>
          <img class="pokemon-img" :src="img2" alt="no hay imagen">
          <h1 class="texto2">{{ texto2 }}</h1>
        </div>
        <div>
          <img class="pokemon-img" :src="img3" alt="no hay imagen">
          <h1 class="texto3">{{ texto3 }}</h1>
        </div>
      </div>
      <button class="botonjugar" @click="iniciarJuego">JUGAR</button>
    </div>
    <div class="cuadroJugar" v-if="intento === 5">
      <h1 class="mensaje" :style="{ color: puntaje < 10 ? 'red' : 'blue' }"> Puntaje: {{puntaje}} 
        {{ mensaje }}</h1>
      <button class="botonjugar" @click="reiniciarJuego">Nuevo Juego</button>
    </div>
    
  </div>
</div>
</template>

<script>
import Img250 from './assets/imagen250.png'
export default{
  data(){
    return{
      puntaje: 0,
      intento: 0,
      num1: null,
      num2: null,
      num3: null,
      img1: Img250,
      img2: Img250,
      img3: Img250,
      texto1: "xxxxxxx",
      texto2: "xxxxxxx",
      texto3: "xxxxxxx",
      juegoIniciado: false,
      mensaje: "",
    }
  },
  methods: {
    randomNumber() {
      return Math.floor(Math.random() * 4) + 1;
    },
    fetchPokemonName(id) {
      fetch(`https://pokeapi.co/api/v2/pokemon/${id}`)
        .then(response => response.json())
        .then(data => {
          if (id === this.num1) {
            this.texto1 = data.name;
          } else if (id === this.num2) {
            this.texto2 = data.name;
          } else {
            this.texto3 = data.name;
          }
        });
    },
    iniciarJuego() {
      if (this.intento < 5) {
        this.juegoIniciado = true;
        this.intento++;
        this.num1 = this.randomNumber();
        this.num2 = this.randomNumber();
        this.num3 = this.randomNumber();
        this.fetchPokemonName(this.num1);
        this.fetchPokemonName(this.num2);
        this.fetchPokemonName(this.num3);
        if (this.num1 === this.num2 && this.num2 === this.num3) {
          this.puntaje += 5;
        } else if (this.num1 === this.num2 || this.num2 === this.num3 || this.num1 === this.num3) {
          this.puntaje += 2;
        }
      }
      if (this.intento === 5) {
        if (this.puntaje < 10) {
          this.mensaje = "\nHas utilizado tus 5 intentos \nEl juego ha terminado, intentalo nuevamente";
        } else {
          this.mensaje = "\nFelicitaciones has ganado un premio de $10.000,00";
        }
      }
    },
    reiniciarJuego() {
      this.intento = 0;
      this.puntaje = 0;
      this.juegoIniciado = false;
      this.mensaje = "";
        this.img1= Img250;
        this.img2= Img250;
        this.img3= Img250;

        this.texto1="xxxxxxx";
        this.texto2="xxxxxxx";
        this.texto3="xxxxxxx";
      
    }
  },
  watch: {
    num1() {
      this.img1 = `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/${this.num1}.svg`;
    },
    num2() {
      this.img2 = `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/${this.num2}.svg`;
    },
    num3() {
      this.img3 = `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/${this.num3}.svg`;
    }
  }
}
</script>

<style>
.pantalla {
  display: grid;
  grid-template-columns: repeat(3, 200px);
  grid-auto-rows: min-content; 
  grid-gap: 10px;
  justify-content: center;
  align-items: center;
  
}

.pokemon-img {
  width: 150px;
  height: 150px;
}

.image {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding-left: 20px;
}

.valores{
  display: flex;
  justify-content: left;
  gap: 300px;
  padding: 20px;
}

.botonjugar{
  display: block;
  margin: 20px auto 0;
  padding: 10px 50px;
  border: 4px solid #000;
  font-size: 1.2em;
  text-align: center;
  margin-bottom: 10px;
}

.mensaje {
  font-size: 3em;
  text-align: center;
  white-space: pre-line;
  
}
.cuadroJugar{
  border: 2px solid #000;
  padding: 30px 50px;
}
.general{
  border: 2px solid #000;
 
  justify-content: center;

}
</style>