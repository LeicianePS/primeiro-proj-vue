<template>
  <div class="main-container">
    <h1>Filtrar Pedidos:</h1>
    <FiltraPedidos/>
  </div>
</template>

<script>
import FiltraPedidos from '../components/FiltraPedidos.vue';

export default {
    name: "PedidosFilterView",
    components: {
      FiltraPedidos
    }
}
</script>
