<script setup>
import { formatearCantidad, formatearFecha } from "../helpers/index";
import IconoAhorro from "../assets/img/icono_ahorro.svg";
import IconoCasa from "../assets/img/icono_casa.svg";
import IconoComida from "../assets/img/icono_comida.svg";
import IconoGastos from "../assets/img/icono_gastos.svg";
import IconoOcio from "../assets/img/icono_ocio.svg";
import IconoSalud from "../assets/img/icono_salud.svg";

const diccionarioIconos = {
  ahorro: IconoAhorro,
  comida: IconoComida,
  casa: IconoCasa,
  gastos: IconoGastos,
  ocio: IconoOcio,
  salud: IconoSalud,
};

const emits=defineEmits(['seleccionar-gasto'])

const props = defineProps({
  gasto: {
    type: Object,
    required: true,
  },
});
</script>
<template>
  <article class="gasto sombra">
    <div class="contenido">
      <img
        :src="diccionarioIconos[gasto.categoria]"
        class="icono"
        alt="icono gasto"
      />
      <div class="detalles">
        <p class="categoria">{{ gasto.categoria }}</p>
        <p 
          v-on:click="$emit('seleccionar-gasto' , gasto.id)"
          class="nombre">{{ gasto.nombre }}</p>
        <p class="fecha">
          Fecha: <span>{{ formatearFecha(gasto.fecha) }}</span>
        </p>
      </div>
    </div>
    <p class="cantidad">{{ formatearCantidad(gasto.cantidad) }}</p>
  </article>
</template>

<style scoped>
.gasto {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 2rem;
}
.icono {
  width: 60px;
  height: 60px;
}

.contenido{
  display: flex;
  gap: 2rem;
  align-items: center;
}

.categoria {
  color: var(--gris);
  font-size: 1.25rem;
  text-transform: uppercase;
  font-weight: 900;
}

.nombre {
  color: var(--gris-oscuro);
  font-size: 2rem;
  font-weight: 700;
  text-transform: capitalize;
  cursor: pointer;
}

.fecha {
  font-size: 1.65rem;
  font-weight: 900;
  color: var(--gris-oscuro);
}

.fecha span{
  font-weight: 500;
}

.cantidad{
  font-weight: 900;
  font-size: 2rem;
}
</style>
