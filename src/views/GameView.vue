<template lang="">
      <img class="image" :src="image[image_index].image_url" alt="">
    <main class="container">
        <section class="quiz-container">
            <h3>Categoria: {{preguntas[question_index].category.name}}</h3>
            <p>Pregunta: {{preguntas[question_index].pregunta}}</p>
            <ul>
                <li class="option" v-for="option in preguntas[question_index].option" :key="option">
                <label class="option-label">
                    <input class="option-input" type="radio" name="option" :value="option" v-model="opcion">
                        <span class="option-text">{{option.option}}</span>
                </label>
                </li>
            </ul>
            <button @click="getOption">Submit</button>
        </section>
      
    </main>
</template>
<script >
import axios from 'axios';
import {images} from '@/data/images.js'
let QUESTION_URL = 'http://127.0.0.1:8000/api/preguntasWithOptions'
export default {
    data() {
        return {
            preguntas: [],
            options: [],
            question_index: 0,
            image_index: 0,
            opcion: "",
            puntaje: 0,
            image: images
        }
    },
    methods: {
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
            } else if (opcion == "") {
                alert('porfavor selecciona una respuesta')
            } else {
                alert('respuesta incorrecta')
                this.showImage()
                this.nextQuestion()
            }
        },
        guardarPuntaje() {
            const puntaje = this.puntaje
            localStorage.setItem('puntaje', puntaje)
            console.log(`puntaje: ${puntaje}`)
        },
        showImage() {
            this.image_index++
             if (this.image_index >= this.image.length - 1) {
                alert('Fin de la trivia')
                
             }
            console.log(this.image[0])


        },

    },
    created() {
        this.getQuestions()
    }
}
</script>
<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Josefin+Sans&display=swap');

.image{
    width: 100px;
    height: 100px;
}
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
.option-input::before{
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
 box-shadow: 2px 2px 4px rgba(0,0,0,0.4);
 background: dodgerblue;
 color: white;
 border-radius: 4px;
}

button:hover {
 background: rgb(2,0,36);
 background: linear-gradient(90deg, rgba(30,144,255,1) 0%, rgba(0,212,255,1) 100%);
}

button:active {
 transform: translate(0em, 0.2em);
}
</style>