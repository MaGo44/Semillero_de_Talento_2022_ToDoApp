<template>
  <v-app>
    <TheHeader/>
    <v-container>
      <div class="main-option">
            <h1>Esta pagina no existe...</h1>
            <v-img 
                :src="require('@/assets/images/auxiliar.png')" 
                alt="fe" 
                class="image"
                max-width="300"
                eager
            >
            </v-img>
      </div>
    </v-container>
  </v-app>
</template>
<style scoped>
.main-option{
  margin-top: 15rem;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
