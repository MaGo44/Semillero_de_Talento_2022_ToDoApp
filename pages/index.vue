/* 
    Descripcion:
    Pagina principal que envia al usuario a la lista de tareas 
*/
<template>
  <v-app>
     <!-- El componente de encabezado -->
    <TheHeader/>
    <v-container>
      <div class="main-option">
        <!-- Link hacia la pagina de tareas -->
        <NuxtLink to="/tasks">
          <v-btn
            class="ma-2"
          >
            Empezar ahora
          </v-btn>
        </NuxtLink>
      </div>
    </v-container>
  </v-app>
</template>

<script>
export default {
  name: 'IndexPage'
}
</script>
<style scoped>
.main-option{
  margin-top: 15rem;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>

