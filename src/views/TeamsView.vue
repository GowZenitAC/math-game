<template>
    <main class="container">
        <h1 class="title">Teams</h1>
        <section>
            <p class="subtitle">Porfavor Selecciona tu equipo</p>
            <div class="select-teams">
                <select name="" id="">
                    <option value="">Selecciona tu equipo</option>
                </select>
                <button @click="next">Seleccionar</button>
            </div>
        </section>
    </main>
</template>
<script>
export default {

    methods:{
        next(){
            this.$router.push({ name: 'game' })
        }
    }
}
</script>
<style scoped>
.container {
    background-color: #145381;
    margin: 2em 0 2em 0;
    width: 100%;
    height: 65dvh;
    box-shadow: inset 0 0 1em -0.5em #f6f6f6;
    border-radius:15px;
    
}

.title {
    text-align: center;
    color: white;
    font-size: 3rem;
    margin: 0 3rem 0 3rem;
}
.subtitle{
    text-align: center;
    margin-top: 1em;
    color: white;
    font-size: 20px;
    animation: shake-bottom 0.8s cubic-bezier(0.455, 0.030, 0.515, 0.955) both infinite;

}
.select-teams{
    display: flex;
    justify-content: center;
    margin-top: 1rem;
}
.select-teams select{
    outline: none;
    border: none;
    border-radius: 5px;
    padding: 1em;

}
.select-teams select:hover{
    outline: none;

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