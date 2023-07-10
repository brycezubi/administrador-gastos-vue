<script setup>
import { computed, ref } from "vue";
import Close from "../assets/img/cerrar.svg";
import Alerta from "./Alerta.vue";

const error = ref("");

const emits = defineEmits([
  "close-modal",
  "update:nombre",
  "update:cantidad",
  "update:categoria",
  "crear-gasto",
  "eliminar-gasto"
]);

const props = defineProps({
  modal: {
    type: Object,
    required: true,
  },
  id: {
    type: [String, null],
    required: true,
  },
  nombre: {
    type: String,
    required: true,
  },
  cantidad: {
    type: [String, Number],
    required: true,
  },
  categoria: {
    type: String,
    required: true,
  },
  disponible: {
    type: Number,
    required: true,
  },
});

const old = props.cantidad;

const isEdit = computed(() => {
  return props.id;
});

const validarFormulario = () => {
  const { cantidad, categoria, nombre, disponible, id } = props;
  if ([nombre, cantidad, categoria].includes("")) {
    error.value = "Todos los campos son obligatorios";
    setTimeout(() => {
      error.value = "";
    }, 1500);
    return;
  }

  if (cantidad <= 0) {
    error.value = "Cantidad no Valida";
    setTimeout(() => {
      error.value = "";
    }, 1500);
    return;
  }

  if (id) {
    //Tomar en cuenta el gasto ya realizado
    if (cantidad > old + disponible) {
      error.value = "Presupuesto excedido";
      setTimeout(() => {
        error.value = "";
      }, 1500);
      return;
    }
  } else {
    if (cantidad > disponible) {
      error.value = "Superaste tu Presupuesto";
      setTimeout(() => {
        error.value = "";
      }, 1500);
      return;
    }
  }

  emits("crear-gasto");
};
</script>
<template>
  <div class="modal">
    <div
      v-on:click="$emit('close-modal')"
      class="cerrar-modal"
    >
      <img
        :src="Close"
        alt="icono close modal"
      />
    </div>

    <form
      v-on:submit.prevent="validarFormulario"
      :class="[modal.animar ? 'animate' : 'cerrar']"
      class="contenedor formulario"
    >
      <legend>{{ isEdit ? "Actualizando Gasto" : "Añadir Gasto" }}</legend>
      <Alerta v-if="error">
        {{ error }}
      </Alerta>
      <div class="campo">
        <label for="name">Nombre Gasto:</label>
        <input
          type="text"
          placeholder="conciertos , salidas..."
          id="name"
          :value="nombre"
          @input="$emit('update:nombre', $event.target.value)"
        />
      </div>
      <div class="campo">
        <label for="cantidad">Cantidad Gasto:</label>
        <input
          type="number"
          placeholder="$50.00"
          id="cantidad"
          :value="cantidad"
          @input="$emit('update:cantidad', +$event.target.value)"
        />
      </div>
      <div class="campo">
        <label for="categoria">Nombre Gasto:</label>
        <select
          name="categoria"
          id="categoria"
          :value="categoria"
          @input="$emit('update:categoria', $event.target.value)"
        >
          <option value="">--Seleccione--</option>
          <option value="ahorro">Ahorro</option>
          <option value="comida">Comida</option>
          <option value="casa">Casa</option>
          <option value="gastos">Gastos Corrientes</option>
          <option value="ocio">Ocio</option>
          <option value="salud">Salud</option>
        </select>
      </div>
      <input
        class="enviar"
        type="submit"
        :value="[isEdit ? 'Guardar Cambios' : 'Añadir Gasto']"
      />
      <div 
        v-if="isEdit"
        class="centrar">
        <button 
          v-on:click="$emit('eliminar-gasto',id)"
          class="btn-delete">Eliminar Gasto</button>
      </div>
    </form>
  </div>
</template>

<style scoped>
.modal {
  position: absolute;
  background-color: rgb(0 0 0 / 0.9);
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}

.cerrar-modal {
  cursor: pointer;
  position: absolute;
  top: 3rem;
  right: 3.5rem;
}

.cerrar-modal img {
  width: 30px;
  height: 30px;
}

.formulario {
  display: flex;
  flex-direction: column;
  gap: 2rem;

  margin-top: 10rem;
}

.formulario legend {
  text-align: center;
  color: var(--white);
  font-size: 3rem;
  font-weight: 900;
}

.campo {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.campo label {
  color: var(--white);
  font-weight: 700;
}

.campo input {
  padding-inline: 2rem;
  padding-block: 0.85rem;
  border-radius: 0.25rem;
  border: none;
  outline: none;
}

.campo select {
  text-align: center;
  padding-block: 0.75rem;
  color: var(--black);
  font-weight: 700;
}

.enviar {
  background-color: var(--azul);
  padding-block: 1rem;
  outline: none;
  border: none;
  color: var(--white);
  border-radius: 0.25rem;
  transition: all 0.3s ease;
}

.enviar:hover {
  cursor: pointer;
  letter-spacing: 1px;
  opacity: 0.9;
}

.animate {
  transition-property: all;
  transition-duration: 300ms;
  transition-timing-function: ease-in;
  opacity: 0;
}

.formulario.animate {
  opacity: 1;
}
.formulario.cerrar {
  opacity: 0;
}

.centrar {
  display: flex;
  justify-content: center;
}

.btn-delete {
  background-color: #ef4444;
  padding: 0.85rem;
  width: 400px;
  border-radius: 0.5rem;
  border: none;
  transition: all 0.3s ease-in;
  font-weight: 700;
  color: var(--white);
  margin-top: 5rem;
}

.btn-delete:hover {
  cursor: pointer;
  opacity: 0.9;
  letter-spacing: 1px;
}
</style>
