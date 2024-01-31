<template lang="">
    <main>
        
    </main>
</template>
<script>
import axios from 'axios';
let QUESTION_URL = 'http://127.0.0.1:8000/api/preguntas'
export default {
    data(){
        return {
            preguntas:[]
        }
    },
    methods:{
       async getQuestions(){
          try {
            const response = await axios.get(QUESTION_URL)
            const data = await response.data
            this.preguntas = data
            console.log(data);
          } catch (error) {
            throw error
          }
        }
    },
    mounted(){
        this.getQuestions()
    }
}
</script>
<style lang="">
    
</style>