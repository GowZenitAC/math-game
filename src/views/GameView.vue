<template lang="">
    <p class="letters">Palabra Secreta: {{ palabra }}</p>
    <h3 class="category">CATEGORÍA: {{preguntas[question_index].category.name}}</h3>
    <main class="container">
        <section class="quiz-container">
            <div class="timer-container">
                <p class="timer-text">Tiempo Restante: {{ formatTime }}</p>
            </div>
            <p class="question">Pregunta: <span v-html="renderQuestion(preguntas[question_index].pregunta)"></span></p>
            <img class="question-image" v-if="preguntas[question_index].imagen_pregunta" :src="getImageUrl(preguntas[question_index].imagen_pregunta)" alt="Imagen de la pregunta">
            <img class="image" :src="image[image_index].image_url" alt="">
            <ul>
                <li class="option" v-for="option in preguntas[question_index].option" :key="option">
                <label :class="{ 'option-label-success': correct_aswer && option_selected === option, 'option-label-fail': !correct_aswer && option_selected === option}" class="option-label">
                    <input class="option-input" type="radio" name="option" :value="option" v-model="opcion">
                    <span class="option-text" v-html="renderOption(option.option)"></span>
                </label>
                </li>
            </ul>
            <button v-if="!finished" @click="getOption">Submit</button>
            <button v-else @click="finishGame">Finish Game</button>
        </section>
    </main>
</template>
<script >
import axios from 'axios';
import katex from 'katex';
import 'katex/dist/katex.min.css';
import { images } from '@/data/images.js'
import Swal from 'sweetalert2'
import 'sweetalert2/dist/sweetalert2.min.css';
let QUESTION_URL = 'http://127.0.0.1:8000/api/preguntasWithOptions'
let RESULT_URL = 'http://127.0.0.1:8000/api/resultados'
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
            startTime: null,
            endTime: null,
            palabra: "",
            palabras: ['Matemáticas', 'Algebra', 'Baldor', 'Ángulo'],
            image: images,
            image_index: 0,
            BASE_URL: 'https://adminmathday.com/',
            correct_aswer: null,
            option_selected: null,
            finished: false,
            finishedTime: null
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
            this.startTime = new Date();
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
            this.correct_aswer = null;
            this.option_selected = null;
            if (this.question_index >= this.preguntas.length - 1) {
                this.finished = true; // Si es la última pregunta, establecer finished a true
            }

        },
        getOption() {
            const opcion = this.opcion
            console.log(opcion)
            if (opcion.points > 0) {
                this.correct_aswer = true
                this.option_selected = this.opcion
                setTimeout(() => {
                    this.nextQuestion()
                }, 1200)

                this.puntaje = parseInt(this.puntaje) + parseInt(opcion.points);
                this.guardarPuntaje()
                if (this.palabra.length < this.palabras[this.word_index].length) {
                    this.palabra += this.palabras[this.word_index][this.palabra.length];
                    console.log(this.palabra)
                    if (this.palabra === this.palabras[this.word_index]) {
                        // Si la palabra actual es igual a la palabra en el índice actual del array
                        Swal.fire({
                            title: `¡Felicidades!, Descubriste la palabra: ${this.palabra}`,
                            width: 600,
                            padding: "3em",
                            color: "#716add",
                            background: "#fff url(/images/trees.png)",
                            backdrop: `
                                rgba(0,0,123,0.4)
                                url("/images/nyan-cat.gif")
                                left top
                                no-repeat`
                        }).then(() => {
                            this.word_index++; // Pasar a la siguiente palabra
                            this.palabra = ""; // Reiniciar la palabra actual
                        })

                    }
                }

            } else if (opcion == "") {
                Swal.fire({
                    title: "Porfavor...",
                    text: "Selecciona una respuesta",
                    icon: "warning"
                });
            } else {
                // alert('respuesta incorrecta')
                setTimeout(() => {
                    this.nextQuestion()
                    this.showImage()
                }, 1200)
                this.option_selected = this.opcion
            }
        },
        guardarPuntaje() {
            const puntaje = this.puntaje
            localStorage.setItem('puntaje', puntaje)
            console.log(`puntaje: ${puntaje}`)
        },
        saveTime() {
            this.endTime = new Date(); // Guardar la hora de finalización
            const elapsedTime = (this.endTime - this.startTime) / 1000; // Tiempo transcurrido en segundos

            const hours = Math.floor(elapsedTime / 3600);
            const minutes = Math.floor((elapsedTime % 3600) / 60);
            const seconds = Math.floor(elapsedTime % 60);
            const formattedTime = `${hours}:${minutes}:${seconds}`
            this.finishedTime = formattedTime
            localStorage.setItem('tiempo', formattedTime)
            console.log(formattedTime);
        },
        showImage() {
            this.image_index++;
            if (this.image_index >= this.image.length - 1) {



                Swal.fire({
                    icon: "error",
                    title: `Oops...Se te han descontado puntos`,
                    text: `No descubriste la palabra: ${this.palabras[this.word_index]}`,
                }).then(() => {
                    this.palabra = "";
                    this.word_index++;
                    this.image_index = 0;
                })
            }
            console.log(this.images[this.image_index]);

        },

        getImageUrl(imagePath) {
            return `${this.BASE_URL}${imagePath}`;
        },
        finishGame() {
           const equipo = localStorage.getItem('equipo',)
          
            this.getOption()
            this.saveTime()
            const res = {
                id_equipo: equipo,
                puntos: this.puntaje,
                tiempo: this.finishedTime
            }
            axios.post(RESULT_URL, res).then(response => {
                console.log(response.data)
            }).catch(error => {
                console.log(error)
            })
            Swal.fire({
                title: "FELICITACIONES!, has terminado satisfactoriamente el juego",
                html: "Tus resultados estan siendo generados...",
                allowOutsideClick: false,
                timer: 5000,
                timerProgressBar: true,
                didOpen: () => {
                    Swal.showLoading();
                },
            }).then((result) => {
                this.$router.push({ name: 'result' })
            });
        }

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

.image {
    width: 150px;
    height: 150px;
    position: absolute;
    left: 6em;
    top: 20em;
}

.category {
    font-family: 'Josefin Sans', sans-serif;
    color: white;
    font-size: 1.3em;
    margin-right: 1em;
    margin-left: 24em;
    display: inline;
}

.question {
    font-family: 'Josefin Sans', sans-serif;
    color: white;
    white-space: wrap;
    width: 40em;
    font-size: 1em;
    text-align: center;
}

.question-image {
    margin: auto;
}

.container {
    background-color: #145381;
    border-radius: 10px;
    width: 100%;
    height: 100%;
    margin: 2px 0 10px 0;
    display: flex;
}

.quiz-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    margin: 1em;
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

.option-label-fail {
    animation: shake-horizontal 0.8s cubic-bezier(0.455, 0.030, 0.515, 0.955) both;
    background-color: rgb(230, 88, 88);
}

.option-label-success {
    animation: shake-vertical 0.8s cubic-bezier(0.455, 0.030, 0.515, 0.955) both;
    background-color: rgb(109, 240, 109);
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
    font-size: 1.89em;
    text-align: left;
    display: inline;
}

/* Animaciones */
@keyframes shake-horizontal {

    0%,
    100% {
        transform: translateX(0);
    }

    10%,
    30%,
    50%,
    70% {
        transform: translateX(-10px);
    }

    20%,
    40%,
    60% {
        transform: translateX(10px);
    }

    80% {
        transform: translateX(8px);
    }

    90% {
        transform: translateX(-8px);
    }
}

@keyframes shake-vertical {

    0%,
    100% {
        transform: translateY(0);
    }

    10%,
    30%,
    50%,
    70% {
        transform: translateY(-8px);
    }

    20%,
    40%,
    60% {
        transform: translateY(8px);
    }

    80% {
        transform: translateY(6.4px);
    }

    90% {
        transform: translateY(-6.4px);
    }
}
</style>