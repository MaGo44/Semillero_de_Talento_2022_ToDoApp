/* 
    Descripcion:
    Componente de encabezado.
*/
<template>
    <div>
        <!-- Barra o menu desplegable -->
        <v-navigation-drawer 
            v-model="isOpen" 
            :mini-variant="isActive" 
            :clipped="isClipped" 
            app 
            floating 
            width="320" 
            color="grey darken-3"
        >
            <v-layout 
                column 
                justify-centerb 
                fill-height
            >
                <div>
                    <v-list-item>
                        <v-list-item-title 
                            class="text-center white--text"
                        >
                            Semillero de Talento 
                            <br/>
                            <span 
                                class="grey--text"
                            > 
                                2022
                            </span>
                        </v-list-item-title>
                    </v-list-item>
                </div>
                <slot name="drawerContent"/>
            </v-layout>
        </v-navigation-drawer>
        <!-- Boton para abrir y cerrar la barra -->
        <v-app-bar 
            :clipped-left="isClipped" 
            isFixed
            app 
            color="grey darken-3"
        >
            <v-app-bar-nav-icon color="white" @click.stop="isOpen=!isOpen"/>
            <v-toolbar-title v-if="!isOpen" v-text="title" class="white--text"/>
            <v-spacer/>
            <slot name="barSearcher"/>
        </v-app-bar>
    </div>                  
</template>
<script>
export default {
    data() {
        return{
            isClipped:false,
            isOpen:false,
            fixed:false,
            isActive:false,
            title:"Semillero de Talento",
        }
    }
}
</script>