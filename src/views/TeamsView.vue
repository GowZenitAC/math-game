<template>
    <main class="container">
        <h1 class="title">Teams</h1>
        <section>
            <p class="subtitle">Porfavor Selecciona tu equipo</p>
            <div class="select-teams">
                <select name="hola" id="1">
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

/* Estilo del select */
select {
    padding: 10px;
    font-size: 16px;
    border: 2px solid #eec20f; /* Color del borde */
    border-radius: 5px;
    outline: none;
    transition: border-color 0.3s ease;
    width: 200px; /* Ajusta el ancho según tus necesidades */
    cursor: pointer; /* Cambia el cursor al pasar sobre el select */
    color: #f5f0f0;
    background-color: rgb(77, 71, 245);
}

/* Estilo del texto del option */
select option {
    font-size: 14px;
}

/* Estilo del icono desplegable */
.select-teams::after {
    content: '\25BC'; /* Triángulo apuntando hacia abajo como un icono desplegable */
    font-size: 12px;
    position: absolute;
    top: 50%;
    right: 10px;
    transform: translateY(-50%);
    pointer-events: none; /* Evita que el icono afecte a la interacción del usuario con el select */
}

/* Estilo cuando el select está enfocado */
select:focus {
    border-color: #2ecc71; /* Nuevo color del borde cuando está enfocado */
}

/* Estilo cuando el usuario pasa el ratón sobre el select */
select:hover {
    border-color: #2980b9; /* Nuevo color del borde al pasar el ratón sobre el select */
}

/* Estilo para la opción seleccionada */
select option:checked {
    background-color: #3498db;
    color: #fff;
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



