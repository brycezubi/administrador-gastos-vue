<script setup>
import { computed } from 'vue';
import { formatearCantidad } from '../helpers/index';
import CircleProgress from "vue3-circle-progress";
import "vue3-circle-progress/dist/circle-progress.css";

defineEmits(['reset-app'])
const props = defineProps({
  presupuesto:{
    type:Number,
    required:true
  },
  disponible:{
    type:Number,
    required:true
  },
  gastado:{
    type:Number,
    required:true
  }
})

const porcentaje = computed(()=>{
  return parseInt((props.presupuesto - props.disponible)*100 / props.presupuesto)
})
</script>
<template>
  <section class="dos-columnas">
    <div class="contenedor-grafico">
      <p class="porcentaje">{{porcentaje}}%</p>
      <CircleProgress
        :percent="porcentaje"
        :size="250"
        fill-color="#3b82f6"
        empy-color="#f5f5f5"
      />
    </div>

    <div class="contenedor-presupuesto">
      <button
        v-on:click="$emit('reset-app')"
        class="reset-app"
      >
        Resetear App
      </button>

      <p><span>Presupuesto:</span>{{ formatearCantidad(presupuesto) }}</p>
      <p><span>Disponible:</span>{{ formatearCantidad(disponible) }}</p>
      <p><span>Gastado:</span>{{ formatearCantidad(gastado) }}</p>
    </div>
  </section>
</template>
<style scoped>
.dos-columnas {
  display: grid;
  justify-content: center;
  gap: 3rem;
}

@media (min-width: 820px) {
  .dos-columnas {
    grid-template-columns: 1fr 1fr;
    gap: 4.5rem;
    align-items: center;
  }
}

.contenedor-presupuesto {
  width: 100%;
}
.contenedor-presupuesto p {
  font-size: 2.22rem;
  text-align: center;
  color: var(--gris-oscuro);
}
@media (min-width: 820px) {
  .contenedor-presupuesto p {
    text-align: left;
  }
}
.contenedor-presupuesto p span {
  font-weight: 700;
  color: var(--azul);
}

.reset-app {
  background-color: #bd2777;
  border: none;
  padding: 1rem;
  border-radius: 0.5rem;
  color: var(--white);
  font-weight: 700;
  width: 100%;
  transition: all 0.2s ease;
}

.reset-app:hover {
  cursor: pointer;
  letter-spacing: 1px;
  opacity: 0.9;
}

.contenedor-grafico{
  position: relative;
}

.porcentaje{
  position: absolute;
  font-size: 3.5rem;
  color: #005ae0;
  font-weight: 900;
  top: 36%;
  left: 48%;
  transform: translate(-50%, -50%);

}
</style>
