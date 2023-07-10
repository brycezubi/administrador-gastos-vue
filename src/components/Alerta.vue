<template>
  <div class="alerta">
    <slot />
  </div>
</template>
<style>
  .alerta{
    padding: 1rem;
    font-weight: 900;
    text-align: center;
    background-color: var(--white);
    font-size: 1.65rem;
    text-transform: uppercase;
    border-left: .5rem solid #b91c1c;
    color: #b91c1c;
    margin-bottom: 2rem;
  }
</style>