
<template>
   <img  class="logo" src="../../public/assets/images/logom.png" alt="">
    <button class="cambio" @click="toggleCard">alternarTarjeta</button>
    <main class="container">
      <h1 class="title">Teams</h1>
      <div class="card" :class="{ 'flipped': flipped }">
  
        <div class="front">
          <h1 class="title">preparatoria</h1>
          <section>
            <p class="subtitle">Por favor Selecciona tu equipo</p>
            <div class="select-teams">
              <div class="select-container">
                <select name="team" id="1" v-model="equipoSelected" @change="saveTeam">
                  <option value="" disabled selected>Selecciona tu equipo</option>
                  <option v-for="equipo in equipos" :key="equipo.id" :value="equipo.id">{{ equipo.nombre }}
                  </option>
                </select>
              </div>
              <div class="botom-container">
                <button @click="next">Aceptar</button>
              </div>
            </div>
          </section>
        </div>
  
        <div class="back ">
          <h1 class="title">utc</h1>
          <section>
            <p class="subtitle">Por favor Selecciona tu carrera</p>
            <div class="select-teams">
              <div class="select-container">
                <select name="team" id="1" v-model="carrerasSelected" @change="savecarreras">
                  <option value="" disabled selected> Selecciona tu carrera</option>
                  <option v-for="carrera in carreras" :key="carrera.id" :value="carrera.id">{{ carrera.carrera2 }}
                  </option> 
                </select>
                <select name="team" id="2" v-model="equipotsuSelected" @change="saveEquipo">
                  <option value="" disabled selected>Selecciona tu equipo</option>
                  <option v-for="equipotsu in equiposFiltradosPorCarrera " :key="equipotsu.id" :value="equipotsu.id">{{ equipotsu.nombretsu }}
                  </option>
                </select>
              </div>
  
              <div class="botom-container">
                <button @click="next">Aceptar</button>
              </div>
            </div>
          </section>
        </div>
      </div>
    </main>
  </template>
  
  <script>
 
  import axios from 'axios';
  
  let EQUIPOS_URL = 'https://adminmathday.com/api/apiequipos';
  let CARRERAS_URL = 'https://adminmathday.com/api/carreras';
  let TEAMS_URL = 'https://adminmathday.com/api/equiposTSU';
  
  export default {
    data() {
      return {
        flipped: false,
        equipos: [],
        equipoSelected: '',
        carreras: [],
        carrerasSelected: '',
        equipotsu: [],
        equipotsuSelected: '',
      };
    },
    computed: {
      // Filtra los equipos basados en la carrera seleccionada
      equiposFiltradosPorCarrera() {
      if (!this.carrerasSelected) {
        return [];
      }
      return this.equipotsu.filter(equipotsu => equipotsu.id_carrera === this.carrerasSelected.toString());
    }

    },
    methods: {
      next() {
        this.$router.push({ name: 'game' });
      },
  
      toggleCard() {
        this.flipped = !this.flipped;
      },
  
      async obtenerEquipos() {
        try {
          const response = await axios.get(EQUIPOS_URL);
          this.equipos = response.data;
        } catch (error) {
          console.error('Error al obtener equipos:', error);
        }
      },
  
      async obtenercarreras() {
        try {
          const response = await axios.get(CARRERAS_URL);
          this.carreras = response.data;
        } catch (error) {
          console.error('Error al obtener carreras:', error);
        }
      },
  
      async obtenerequipostsu() {
        try {
          const response = await axios.get(TEAMS_URL);
          this.equipotsu = response.data;
        } catch (error) {
          console.error('Error al obtener equipos TSU:', error);
        }
      },
  
      saveTeam() {
        localStorage.setItem('equipo', this.equipoSelected);
      },
  
      savecarreras() {
        localStorage.setItem('carreras', this.carrerasSelected);
       
      },
  
      saveEquipo() {
        localStorage.setItem('equipotsu', this.equipotsuSelected);
      },
    },
    created() {
      this.obtenerEquipos();
      this.obtenercarreras();
      this.obtenerequipostsu();
    },
  };
  </script>
  
<style scoped>
.container {
    background-color:#3d9fbf;
    margin: 2em 0 2em 0;
    width: 100%;
    height: 47.9dvh;
    box-shadow:  inset 0 0 1em -0.5em #d2a70b, /* Sombra interior amarilla */
        0 0 0em 0.0em rgb(247, 200, 71);
    border-radius: 15px;
    border: #e8c30c 1px s;
    position: relative;
    top: -60px;
}
.logo{
        position: relative;
        left:77%;
        width:10rem;
        top: -100px;
  
    }
 button {
    left: 60%;
    position: relative;
    padding: 1.3em 3em;
  font-size: 12px;
  text-transform: uppercase;
  letter-spacing: 2.5px;
  font-weight: 500;
  color: #000;
  background-color: #fff;
  border: none;
  border-radius: 45px;
  box-shadow: 0px 8px 15px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease 0s;
  cursor: pointer;
  outline: none;
  top: -50px;
  
}

button:hover {
  background-color: #6cb91f;
  box-shadow: 0px 15px 20px rgba(46, 229, 205, 0.4);
  color: #fff;
  transform: translateY(-7px);
}

button:active {
  transform: translateY(-1px);
}

.card {
    position: relative;
  width: 50%;
  height: 100%;
 left: 25%;
  transform-style: preserve-3d;
  transition: transform 0.8s;

}
.card .front,
.card .back {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
}

.card .back {
  transform: rotateY(180deg);
}

.card .front {
  transform: rotateY(0deg);
}

.card.flipped .front {
  transform: rotateY(180deg);
}

.card.flipped .back {
  transform: rotateY(0deg);
}

.title {
    text-align: center;
    color: white;
    font-size: 2.5rem;
    margin: 0 3rem 0 3rem;
}

.subtitle {
    text-align: center;
    margin-top: 0.5em;
    margin-bottom: 1em;
    color: white;
    font-size: 20px;
    animation: shake-bottom 0.8s cubic-bezier(0.455, 0.030, 0.515, 0.955) both infinite;

}

.select-teams {
    display: flex;
    justify-content: space-evenly;
    align-items: center;
}

/* Estilo del select */
select {
    padding: 10px;
    font-size: 16px;
    border: 2px solid #1fb962;
    /* Color del borde */
    border-radius: 5px;
    outline: none;
    transition: border-color 0.3s ease;
    width: 200px;
    margin-right: 10px;
    /* Ajusta el ancho según tus necesidades */
    cursor: pointer;
    /* Cambia el cursor al pasar sobre el select */
    color: #0b1ef1;
    background-color:#fff;
}

/* Estilo del texto del option */
select option {
    font-size: 14px;
}

/* Estilo del icono desplegable */
.select-teams::after {
    /* content: '\25BC';
    Triángulo apuntando hacia abajo como un icono desplegable */
    font-size: 12px;
    position: absolute;
    top: 50%;
    right: 10px;
    transform: translateY(-50%);
    pointer-events: none;
    /* Evita que el icono afecte a la interacción del usuario con el select */
}

/* Estilo cuando el select está enfocado */
select:focus {
    border-color: #2ecc71;
    /* Nuevo color del borde cuando está enfocado */
}

/* Estilo cuando el usuario pasa el ratón sobre el select */
select:hover {
    border-color: #e8c30c;
    /* Nuevo color del borde al pasar el ratón sobre el select */
}

/* Estilo para la opción seleccionada */
select option:checked {
    background-color: #3498db;
    color: #f40b0b;
}

/* Animación para el cambio de color del option al pasar el ratón */
@keyframes colorChange {
    0% {
        background-color: #3498db;
        color: #fff;
    }

    50% {
        background-color: #2ecc71;
        color: #fff;
    }

    100% {
        background-color: #3498db;
        color: #fff;
    }
}


/* Aplica la animación a los options al pasar el ratón */
select option:hover {
    animation: colorChange 1s ease infinite;
}

.select-container2 {
    flex: 1;
    /* Toma el espacio disponible */
    margin-right: 10px;
    /* Margen entre el select y el botón */
}

/* botom */
.button-container {
    flex: 0;
    /* No toma espacio adicional */
}

 .botom-container  button {
    appearance: button;
    left: 10px;
    background-color: #1899D6;
    border: solid transparent;
    border-radius: 16px;
    border-width: 0 0 4px;
    box-sizing: border-box;
    color: #FFFFFF;
    cursor: pointer;
    display: inline-block;
    font-size: 15px;
    font-weight: 700;
    letter-spacing: .8px;
    line-height: 20px;
    margin: 0;
    outline: none;
    overflow: visible;
    padding: 13px 19px;
    text-align: center;
    text-transform: uppercase;
    touch-action: manipulation;
    transform: translateZ(0);
    transition: filter .2s;
    user-select: none;
    -webkit-user-select: none;
    vertical-align: middle;
    white-space: nowrap;
    top: 0;
}

 .botom-container button:after {
    background-clip: padding-box;
    background-color: #1CB0F6;
    border: solid transparent;
    border-radius: 16px;
    border-width: 0 0 4px;
    bottom: -4px;
    content: "";
    left: 0;
    position: absolute;
    right: 0;
    top: 0;
    z-index: -1;
}


 button:focus {
    user-select: auto;
}

 button:hover:not(:disabled) {
    filter: brightness(1.1);
}
 button:disabled {
    cursor: auto;
}
 button:active:after {
    border-width: 0 0 0px;
}

button:active {
    padding-bottom: 10px;
}

/*Animaciones*/
@keyframes shake-bottom {

    0%,
    100% {
        transform: rotate(0deg);
        transform-origin: 50% 100%;
    }

    10% {
        transform: rotate(2deg);
    }

    20%,
    40%,
    60% {
        transform: rotate(-4deg);
    }

    30%,
    50%,
    70% {
        transform: rotate(4deg);
    }

    80% {
        transform: rotate(-2deg);
    }

    90% {
        transform: rotate(2deg);
    }
}
</style>



