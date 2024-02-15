<template>
    <main class="container">
        <h1 class="title">Teams</h1>
        <section>
            <p class="subtitle">Porfavor Selecciona tu equipo</p>
            <div class="select-teams">
                <div class="select-container">
                    <select name="team" id="1" v-model="equipoSelected" @change="saveTeam">
                        <option value="" disabled selected>Selecciona tu equipo</option>
                        <option v-for="equipo in equipos" :key="equipo.id" :value="equipo.id">{{ equipo.nombre }}</option>
                    </select>
                </div>
                <div class="botom-container">
                    <button @click="next">Seleccionar</button>
                </div>
            </div>
        </section>
    </main>
</template>
<script>
import axios from 'axios';

let EQUIPOS_URL = 'http://127.0.0.1:8000/api/apiequipos';

export default {
    data() {
        return {
            equipos: [],
            equipoSelected: ''
        };
    },
    methods: {
        next() {
            this.$router.push({ name: 'game' });
        },
        async obtenerEquipos() {
            try {
                const response = await axios.get(EQUIPOS_URL);
                this.equipos = response.data;
                console.log(this.equipos);
            } catch (error) {
                throw error;
            }
        },
        saveTeam(){
            localStorage.setItem('equipo', this.equipoSelected)
        }


    },
    created() {
        this.obtenerEquipos(); // Corregido el nombre del método
    },
};

</script>
<style scoped>
.container {
    background-color: #145381;
    margin: 2em 0 2em 0;
    width: 100%;
    height: 65dvh;
    box-shadow: inset 0 0 1em -0.5em #f6f6f6;
    border-radius: 15px;

}

.title {
    text-align: center;
    color: white;
    font-size: 3rem;
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
    justify-content: space-between;

    align-items: center;
}

/* Estilo del select */
select {
    padding: 10px;
    font-size: 16px;
    border: 2px solid #2980b9;
    /* Color del borde */
    border-radius: 5px;
    outline: none;
    transition: border-color 0.3s ease;
    width: 200px;
    /* Ajusta el ancho según tus necesidades */
    cursor: pointer;
    /* Cambia el cursor al pasar sobre el select */
    color: #f5f0f0;
    background-color: rgb(77, 71, 245);
}

/* Estilo del texto del option */
select option {
    font-size: 14px;
}

/* Estilo del icono desplegable */
.select-teams::after {
    content: '\25BC';
    /* Triángulo apuntando hacia abajo como un icono desplegable */
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

.select-container {
    flex: 1;
    /* Toma el espacio disponible */
    margin-right: 10px;
    /* Margen entre el select y el botón */
}

/* botom */
.button-container {
  flex: 0; /* No toma espacio adicional */
}
button {
    appearance: button;
    left: 20px;
    
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
}

button:after {
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

button:main,
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



