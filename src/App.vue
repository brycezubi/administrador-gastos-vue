<script setup>
import NuevoGasto from "./assets/img/nuevo-gasto.svg";
import { uid } from "uid";
import { ref, reactive, watch , computed, onMounted } from "vue";

import Presupuesto from "./components/Presupuesto.vue";
import ControlPresupuesto from "./components/ControlPresupuesto.vue";
import Modal from "./components/Modal.vue";
import Gasto from "./components/Gasto.vue";
import Filtros from "./components/Filtros.vue";


const gastos = ref([]);
const presupuestoBase = ref(0);
const disponible = ref(0);
const gastado = ref(0);
const filtro = ref('')

watch(
  gastos,
  () => {
    const totalGastado = gastos.value.reduce(
      (total, acum) => total + acum.cantidad,
      0
    );
    gastado.value = +totalGastado;
    disponible.value = presupuestoBase.value - totalGastado;
    
    localStorage.setItem('gastos',JSON.stringify(gastos.value))

  },
  {
    deep: true,
  }
);

watch(presupuestoBase,()=>{
  localStorage.setItem('presupuesto',presupuestoBase.value)
})

onMounted(() => {
  const presupuestoStorage = localStorage.getItem('presupuesto')
  const gastoStorage =  localStorage.getItem('gastos')

  if(presupuestoStorage){
    presupuestoBase.value=Number(presupuestoStorage)
    disponible.value=Number(presupuestoStorage)
  }

  if(gastoStorage){
    gastos.value= JSON.parse(gastoStorage)
  }
})

const gasto = reactive({
  id: null,
  nombre: "",
  cantidad: "",
  categoria: "",
  fecha: Date.now(),
});

const modal = reactive({
  mostrar: false,
  animar: false,
});

const tomarPresupuesto = (presupuesto) => {
  presupuestoBase.value = presupuesto;
  disponible.value = presupuesto;
};

const mostrarModal = () => {
  modal.mostrar = true;

  setTimeout(() => {
    modal.animar = true;
  }, 300);
};

const closeModal = () => {
  modal.animar = false;

  setTimeout(() => {
    modal.mostrar = false;
  }, 300);
  resetFields();
};

const crearGasto = () => {
  // Validamos si el gasto ya se encuentra agregado
  const { id } = gasto;
  if (gasto.id) {
    const indice=gastos.value.findIndex( gasto=>gasto.id === id)
    gastos.value[indice] = {...gasto}
  } else {
    // Crea un gasto nuevo
    gastos.value.push({ ...gasto, id: uid() });
  }
  closeModal();
  resetFields();
};

const seleccionarGasto = (id) => {
  const gastoEditar = gastos.value.filter((gasto) => gasto.id === id)[0];
  Object.assign(gasto, gastoEditar);
  mostrarModal();
};

const eliminarGasto = (id)=>{
  const nuevosGastos =  gastos.value.filter( gasto=> gasto.id !== id)
  gastos.value = nuevosGastos
}

const filtrarGastos = computed(()=>{
  if(filtro.value){
    return gastos.value.filter( gasto=> gasto.categoria === filtro.value)
  }
  return gastos.value
}) 

const resetApp = ()=>{
  if(confirm('¿Deseas reiniciar el presupuesto y los gastos?')){
    presupuestoBase.value = 0
    gastos.value=[]
  }
}

const resetFields = () => {
  return Object.assign(gasto, {
    id: null,
    nombre: "",
    cantidad: "",
    categoria: "",
    fecha: Date.now(),
  });
};
</script>

<template>
  <div :class="{ fijar: modal.mostrar }">
    <header>
      <h1>Planificador de Gastos</h1>
      <div class="contenedor contenedor__header sombra">
        <Presupuesto
          v-if="presupuestoBase === 0"
          @tomar-presupuesto="tomarPresupuesto"
        />

        <ControlPresupuesto
          v-else
          v-bind:presupuesto="presupuestoBase"
          v-bind:disponible="disponible"
          v-bind:gastado="gastado"
          @reset-app="resetApp"
        />
      </div>
    </header>
    

    <main
      class="contenedor"
      v-if="presupuestoBase > 0"
    >
      <Filtros 
        v-model:filtro="filtro"
        @filtrar-gastos="filtrarGastos"
      />

      <section class="listado-gastos">
        <h2>{{ filtrarGastos.length > 0 ? "Gastos" : "No hay Gastos" }}</h2>

        <Gasto
          v-if="gastos.length > 0"
          v-for="gasto in filtrarGastos"
          :key="gasto?.id"
          :gasto="gasto"
          @seleccionar-gasto="seleccionarGasto"
        />
      </section>

      <div
        class="crear-gasto"
        v-on:click="mostrarModal"
      >
        <img
          :src="NuevoGasto"
          alt="icono agregar un gasto"
        />
      </div>

      <Modal
        v-bind:modal="modal"
        v-bind:disponible="disponible"
        v-if="modal.mostrar"
        @close-modal="closeModal"
        v-model:nombre="gasto.nombre"
        v-model:cantidad="gasto.cantidad"
        v-model:categoria="gasto.categoria"
        v-model:id="gasto.id"
        @crear-gasto="crearGasto"
        @eliminar-gasto="eliminarGasto"
      />
    </main>
  </div>
</template>

<style>
:root {
  --azul: #3b82f6;
  --white: #fff;
  --gris-claro: #f5f5f5;
  --gris: #94a3b8;
  --gris-oscuro: #64748b;
  --negro: #000;
}

html {
  font-size: 62.5%;
  box-sizing: border-box;
}

*,
*::before,
*::after {
  box-sizing: inherit;
}

body {
  font-size: 1.6rem;
  font-family: "Lato", sans-serif;
  background-color: var(--gris-claro);
}

h1 {
  font-size: 3.85rem;
}
h2 {
  font-size: 3rem;
}

.fijar {
  overflow: hidden;
  height: 100vh;
}

header {
  background-color: var(--azul);
}

header h1 {
  padding: 3rem 0;
  margin: 0;
  text-align: center;
  color: var(--white);
}

.contenedor {
  width: calc(100% - 12.5%);
  margin-inline: auto;
  max-width: 68rem;
}

.contenedor__header {
  margin-top: -5rem;
  transform: translateY(5rem);
  padding: 5rem;
}

.sombra {
  background-color: var(--white);
  box-shadow: 0px 10px 15px -3px rgba(0, 0, 0, 0.1);
  border-radius: 1.25rem;
  padding: 4rem;
}

.crear-gasto {
  width: 60px;
  height: 60px;
  position: fixed;
  bottom: 6rem;
  right: 2.5rem;
}
.crear-gasto img {
  width: 100%;
}
.crear-gasto img:hover {
  cursor: pointer;
}

.listado-gastos {
  margin-top: 5rem;
}

.listado-gastos h2 {
  font-weight: 900;
  color: var(--gris-oscuro);
}
</style>
