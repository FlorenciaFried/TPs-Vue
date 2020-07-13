<template>
    <div class="container">
        <h1 v-if="check === false"> Debe presionar el boton "Generar información"</h1>
    </div>
</template>

<script>
    export default {
        name: 'Charts',

        props:{
            check:{
                type: Boolean,
                required: true,
            },
        },
    }
</script>

<style scoped>
.container{
    text-align: center;
}

.container h1{
    color: #c28b7a;
}

</style>