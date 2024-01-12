<template>
  <div class="container">
    <div class="tarjetas">
      <img :src="pokemon.imagen" alt="No se puede visualizar" />
      <p>{{ pokemon.respuesta || "XXXXXXXX" }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    pokemon: {
      type: Object,
      required: true,
    },
  },
};
</script>

<style>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
}

img {
  width: 200px;
  height: 200px;
}

.tarjetas {
  display: grid;
  grid-template-rows: repeat(2);
}

p{
  font-weight: bold;
}
</style>
