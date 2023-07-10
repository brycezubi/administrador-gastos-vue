<script setup>
import { ref } from "vue";
import Alerta from "./Alerta.vue";

  const presupuesto = ref(0);
  const error =  ref('')

  const emit = defineEmits(['tomar-presupuesto'])

  const definirPresupuesto = () => {
    if (presupuesto.value <= 0 || presupuesto.value === '' ) {
      error.value='Presupuesto no Valído';
      setTimeout(() => {
        error.value=''
      }, 1000);
      return;
    }
    emit('tomar-presupuesto' , presupuesto.value)
  };
</script>

<template>
  <form
    v-on:submit.prevent="definirPresupuesto"
    class="presupuesto"
  >
    <Alerta v-if="error">
      {{error}}
    </Alerta>

    <div class="campo">
      <label for="presupuesto">Definir Presupuesto</label>
      <input
        class="presupuesto__input"
        type="number"
        id="presupuesto"
        placeholder="Añade tu presupuesto"
        v-model="presupuesto"
      />
    </div>
    <input
      class="enviar"
      type="submit"
      value="Definir Presupuesto"
    />
  </form>
</template>

<style scoped>
.presupuesto {
  width: 100%;
}
.campo {
  display: grid;
  gap: 2rem;
}
.campo label {
  color: var(--azul);
  font-weight: 700;
  font-size: 1.85rem;
  text-align: center;
}
.presupuesto__input {
  background-color: var(--gris-claro);
  border-radius: 1rem;
  border: none;
  padding: 1rem;
  text-align: center;
  font-size: 2rem;
}

.enviar {
  background-color: var(--azul);
  border: none;
  padding: 1rem;
  text-align: center;
  font-size: 1.85rem;
  margin-top: 2rem;
  color: var(--white);
  font-weight: 700;
  width: 100%;
  border-radius: 0.5rem;
  transition: background-color 3000 ease;
}
.enviar:hover {
  cursor: pointer;
  background-color: #1048a4;
}
</style>
