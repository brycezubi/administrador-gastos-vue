<script setup>
  defineProps(['filtro'])
  defineEmits(['update:filtro' , 'filtrar-gastos'])

</script>
<template>
  <form class="sombra contenedor filtros">
    <div class="campo">
        <label for="filtro">Filtrar Gastos:</label>
        <select
          name="filtro"
          id="filtro"
          :value="filtro"
          @change="$emit('update:filtro',$event.target.value)"
          @input="$emit('filtrar-gastos', $event.target.value)"
          class="filtro"
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
  </form>
</template>



<style  scoped>
  .filtros{
    width: 100%;
    margin-top: 8rem;
  }
  
  .campo{
    display: flex ;
    gap: 1rem;
    align-items: center;
  }

  .campo label{
    font-size: 2.5rem;
    font-weight: 700;
    color: var(--gris-oscuro);
  }
  .filtro{
    padding-block: 1rem;
    padding-inline: 1rem;
    border: 1px solid var(--gris);
    border-radius: 0.5rem;
    text-align: center;
    flex: 1;
  }
</style>