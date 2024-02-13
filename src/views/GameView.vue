<template lang="">
    <div></div>
    <p class="letters">Letras: {{ palabra }}</p>
    <main class="container">
        <section class="quiz-container">
            <div class="timer-container">
                <p class="timer-text">Tiempo Restante: {{ formatTime }}</p>
            </div>
            <h3>Categoria: {{preguntas[question_index].category.name}}</h3>
            <p style="color: white; white-space: pre-wrap">Pregunta: <span v-html="renderQuestion(preguntas[question_index].pregunta)"></span></p>
            <p style="color: white">Una fórmula matemática: <span ref="math"></span></p>
           
            <ul>
                <li class="option" v-for="option in preguntas[question_index].option" :key="option">
                <label class="option-label">
                    <input class="option-input" type="radio" name="option" :value="option" v-model="opcion">
                    <span class="option-text" v-html="renderOption(option.option)"></span>
                </label>
                </li>
            </ul>
            <button @click="getOption">Submit</button>
        </section>
    </main>
</template>
<script >
import axios from 'axios';
import katex from 'katex';
import 'katex/dist/katex.min.css';

let QUESTION_URL = 'http://127.0.0.1:8000/api/preguntasWithOptions'
export default {
    data() {
        return {
            preguntas: [],
            options: [],
            question_index: 0,
            word_index: 0,
            opcion: "",
            puntaje: 0,
            timeRemaining: 5400,
            timerInterval: null,
            palabra: "",
            palabras: ['Ma', 'Algebra', 'Baldor', 'Ángulo']
        }
    },
    computed: {
        formatTime() {
            // Convertir segundos a formato HH:MM:SS
            const hours = Math.floor(this.timeRemaining / 3600);
            const minutes = Math.floor((this.timeRemaining % 3600) / 60);
            const seconds = this.timeRemaining % 60;
            return `${this.pad(hours)}:${this.pad(minutes)}:${this.pad(seconds)}`;
        },
    },
    methods: {
        renderQuestion(question) {
            // Dividir el texto en palabras
            const words = question.split(/\s+/);
            // Utilizar KaTeX para renderizar cada palabra y agregarla a un array
            const renderedWords = words.map(word => katex.renderToString(word, { throwOnError: false }));
            // Unir las palabras renderizadas con espacios entre ellas
            return renderedWords.join(' ');
        },
        renderOption(option) {
            // Dividir el texto de la opción en palabras
            const words = option.split(/\s+/);
            // Utilizar KaTeX para renderizar cada palabra y agregarla a un array
            const renderedWords = words.map(word => katex.renderToString(word, { throwOnError: false }));
            // Unir las palabras renderizadas con espacios entre ellas
            return renderedWords.join(' ');
        },
        // Método para iniciar el cronómetro
        startTimer() {
            this.timerInterval = setInterval(() => {
                if (this.timeRemaining > 0) {
                    this.timeRemaining--;
                } else {
                    clearInterval(this.timerInterval);
                    // Aquí puedes agregar lógica adicional cuando el tiempo llega a cero
                    alert('Tiempo agotado!');
                }
            }, 1000); // Actualizar cada segundo
        },

        // Función para agregar ceros a la izquierda si es necesario
        pad(value) {
            return String(value).padStart(2, '0');
        },

        getQuestions() {
            axios.get(QUESTION_URL)
                .then(response => {
                    this.preguntas = response.data
                    console.log(this.preguntas)
                }).catch(error => console.log(error))
        },
        nextQuestion() {
            this.question_index++
            console.log(this.question_index);
            this.opcion = "";

        },
        getOption() {
            const opcion = this.opcion
            console.log(opcion)
            if (opcion.points > 0) {
                alert("Respuesta correcta")
                this.nextQuestion()
                this.puntaje = this.puntaje + opcion.points
                this.guardarPuntaje()
                if (this.palabra.length < this.palabras[this.word_index].length) {
                    this.palabra += this.palabras[this.word_index][this.palabra.length];
                    console.log(this.palabra)
                    if (this.palabra === this.palabras[this.word_index]) {
                        // Si la palabra actual es igual a la palabra en el índice actual del array
                        alert("¡Palabra completada!");
                        this.word_index++; // Pasar a la siguiente palabra
                        this.palabra = ""; // Reiniciar la palabra actual
                    }
                }

            } else if (opcion == "") {
                alert('porfavor selecciona una respuesta')
            } else {
                alert('respuesta incorrecta')
                this.nextQuestion()
            }
        },
        guardarPuntaje() {
            const puntaje = this.puntaje
            localStorage.setItem('puntaje', puntaje)
            console.log(`puntaje: ${puntaje}`)
        },
    },

    created() {
        this.startTimer();
        this.getQuestions()

    },
    beforeDestroy() {
        clearInterval(this.timerInterval);
    }
}
</script>
<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Josefin+Sans&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap');

.container {
    background-color: #145381;
    border-radius: 10px;
    width: 100%;
    height: 100%;
    margin: auto;
    text-align: center;
    margin: 30px 0 10px 0;
    display: flex;
}

.quiz-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    margin: 2em;
    width: 90%;
}

.option {
    list-style: none;
}

.option-input {
    appearance: none;
    margin: 10px;
    border: 2px solid rgba(0, 0, 0, 0.2);
    width: 2.15em;
    height: 2.15em;
    border-radius: 100%;
    position: relative;
    cursor: pointer;
    transform: translateY(-0.075em);
    display: grid;
    place-content: center;
}

.option-input::before {
    content: "";
    width: 0.95em;
    height: 0.95em;
    border-radius: 100%;
    transform: scale(0);
    transition: 120ms transform ease-in-out;
    box-shadow: inset 1em 1em rebeccapurple;
}

.option-input:checked::before {
    transform: scale(1);
}

.option-label {
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: start;
    flex-wrap: wrap;
    width: 40vw;
    max-width: 40vw;
    margin: 10px;
    background-color: beige;
    border-radius: 10px;
    position: relative;
}

.option-text {
    margin: 10px 10px 10px 0px;
    cursor: pointer;
    font-family: 'Josefin Sans', sans-serif;
    color: black;
    font-size: 1.2rem;

}

button {
    font-size: 17px;
    padding: 0.5em 2em;
    border: transparent;
    box-shadow: 2px 2px 4px rgba(0, 0, 0, 0.4);
    background: dodgerblue;
    color: white;
    border-radius: 4px;
}

button:hover {
    background: rgb(2, 0, 36);
    background: linear-gradient(90deg, rgba(30, 144, 255, 1) 0%, rgba(0, 212, 255, 1) 100%);
}

button:active {
    transform: translate(0em, 0.2em);
}

/* diseño del timer */
.timer-container {
    position: fixed;
    top: 10px;
    right: 10px;
    z-index: 1000;
    /* Asegúrate de que el contador esté por encima de otros elementos */
    background-color: rgba(82, 81, 52, 0.5);
    /* Fondo semi-transparente para mayor legibilidad */
    padding: 10px;
    border-radius: 5px;
    color: white;
}

.timer-text {
    font-family: 'Press Start 2P', sans-serif;
    /* Cambia la fuente según tus preferencias */
    font-size: 15px;
    /* Tamaño de fuente */
    font-weight: thin;
    /* Grosor de la fuente */
    letter-spacing: 0px;
    /* Espaciado entre caracteres */
    color: white;
}

.letters {
    margin: 10px 5px 10px 5px;
    font-size: 1.89em;
    text-align: center;

}
</style>