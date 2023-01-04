/* 
    Descripcion:
    Componente de encabezado.
*/
<template>
    <div>
        <!-- Barra para las opciones -->
        <v-toolbar color="grey darken-3">
            <!-- Enlace hacia la pagina de tareas -->
            <NuxtLink to="/tasks">
                <button class="ml-2">
                    <v-toolbar-title v-text="title" class="white--text"/>
                </button>
            </NuxtLink>
        </v-toolbar>
    </div>
</template>
<script>
    export default {
        data() {
            return{
                title:"Semillero de Talento"
            }
        },
    }
</script>