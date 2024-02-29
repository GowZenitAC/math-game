<template lang="">
    <div>
        <p>Tu puntaje es: {{ puntaje }}</p> <p>Tu tiempo es: {{ tiempo }}</p>
    </div>
</template>
<script>
export default {
    mounted() {
        this.getResults()
    },
    data() {
        return {
            puntaje: 0,
            tiempo: 0,

        }
    },
    methods: {
        getResults() {
           this.puntaje = localStorage.getItem('puntaje')
            this.tiempo =  localStorage.getItem('tiempo')
        }
        
    }


}
</script>
<style lang="">
    
</style>