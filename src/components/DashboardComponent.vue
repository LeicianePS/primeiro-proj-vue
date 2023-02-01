<template>
    <div id="burger-table">
        <FiltraPedidos/>
    </div>
</template>

<script>
import MessageComponent from './MessageComponent.vue';
import FiltraPedidos from './FiltraPedidos.vue';
export default {
    name: "DashboardComponent",
    data() {
        return {
            burgers: [],
            burger_id: null,
            status: [],
            msg: null
        };
    },
    components: { 
        FiltraPedidos 
    }
}
</script>

<style scoped>


</style>