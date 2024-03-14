<template lang="">
  <section class="results-test">
    <div class="titulo">
        <IconCheck/>
      <span>¡Felicidades!</span>
      <p>Completaste el test con éxito.</p>
    </div>
    <div class="puntuacion">
      <p>Tu puntuación:</p>
     <span class="puntaje">{{ puntaje }}pts</span>
    </div>
    <div class="tiempo">
      <p>Tiempo empleado:</p>
      <span class="tiempo-empleado">{{ tiempo }}</span>
    </div>
</section>
</template>
<script>
import IconCheck from '@/components/icons/IconCheck.vue';
export default {
  components: {
    IconCheck
  },
  created() {
    this.getResults();
  },
  data() {
    return {
      puntaje: 0,
      tiempo: 0,
    };
  },
  methods: {
    getResults() {
      this.puntaje = localStorage.getItem("puntaje");
      const tiempoString = localStorage.getItem("tiempo");
      this.tiempo = this.convertirTiempo(tiempoString);
    },
    convertirTiempo(tiempoString) {
      const partesTiempo = tiempoString.split(":");
      const horas = parseInt(partesTiempo[0]);
      const minutos = parseInt(partesTiempo[1]);
      const segundos = parseInt(partesTiempo[2]);
      const tiempoTotalSegundos = horas * 3600 + minutos * 60 + segundos;

      if (tiempoTotalSegundos < 60) {
        return `${tiempoTotalSegundos} segundos`;
      } else if (tiempoTotalSegundos < 3600) {
        const minutos = Math.floor(tiempoTotalSegundos / 60);
        return `${minutos} minutos y ${tiempoTotalSegundos % 60} segundos`;
      } else {
        const horas = Math.floor(tiempoTotalSegundos / 3600);
        const minutosRestantes = Math.floor((tiempoTotalSegundos % 3600) / 60);
        return `${horas} horas y ${minutosRestantes} minutos`;
      }
    }
  },
};
</script>
<style>
@import url('https://fonts.googleapis.com/css2?family=Outfit:wght@600&display=swap');

.results-test {
  background-color: #145381;
  border-radius: 10px;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(100px, 1fr));
  grid-template-rows: repeat(auto-fit, minmax(100px, 1fr));
  gap: 10px;
  margin: auto;
  max-width: 900px;
  width: 100%;
  animation: slide-in-left 0.8s cubic-bezier(0.250, 0.460, 0.450, 0.940) both;
}

.titulo {
  grid-column-start: 1;
  grid-column-end: 2;
  grid-row-start: 1;
  grid-row-end: 2;
  display: flex;
  flex-wrap: wrap;
}

.titulo svg {
  margin: 20px 0 10px 10px;
  animation: rotate-in-center 1s cubic-bezier(0.250, 0.460, 0.450, 0.940) 1s both;
}

.titulo span {
  color: white;
  font-size: 2.5em;
  margin: 0.1em;
  font-family: "Outfit", sans-serif;
  font-optical-sizing: auto;
  font-weight: 600;
  font-style: normal;
}

.titulo p {
  color: rgba(255, 249, 249, 0.685);
  font-size: 1.5em;
  font-family: "Outfit", sans-serif;
  margin: 0.2em 0 0.2em 0.5em;
}

.puntuacion {
  grid-column-start: 1;
  grid-column-end: 3;
  display: flex;
  flex-wrap: wrap;
  color: white;
  margin: 0.9em 0 0.9em 1em;
  font-family: "Outfit", sans-serif;
}

.puntuacion p {
  margin: 1.1em 0.5em 1.1em 0.5em;
  font-size: 1.5em;
}

.puntuacion span {
  margin: 0.2em 0.1em 0.2em 0.1em;
  font-size: 2.5em;
}

.tiempo {
  grid-column-start: 3;
  grid-column-end: 2;
  grid-row-start: 1;
  grid-row-end: 2;
  display: flex;
  flex-wrap: wrap;
  flex-direction: column;
  align-items: start;
  justify-content: center;
  font-family: 'Outfit', sans-serif;
  color: white;
}

.tiempo p {
  font-size: 1.5em;

}

.tiempo span {
  font-size: 2.5em;
}

/** Animaciones */
@keyframes rotate-in-center {
  0% {
    transform: rotate(-360deg);
    opacity: 0;
  }

  100% {
    transform: rotate(0);
    opacity: 1;
  }
}

@keyframes slide-in-left {
  0% {
    transform: translateX(-1000px);
    opacity: 0;
  }

  100% {
    transform: translateX(0);
    opacity: 1;
  }
}
</style>
