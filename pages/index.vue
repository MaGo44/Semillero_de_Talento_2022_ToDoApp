<template>
    <v-app>
        <div>
            <v-main>
                <v-container>
                    <v-row>
                        <v-navigation-drawer 
                            v-model="drawer" 
                            :mini-variant="miniVariant" 
                            :clipped="clipped" 
                            app 
                            floating 
                            width="320" 
                            color="grey darken-3">
                            <v-layout 
                                column 
                                justify-centerb 
                                fill-height>
                                <div>
                                    <v-list-item>
                                        <v-list-item-title 
                                            class="text-center white--text">
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
                                <div >
                                    <v-list-item >
                                        <v-btn 
                                            @click=showAllTasks()
                                        >
                                            Todas las tareas
                                        </v-btn>
                                    </v-list-item>
                                </div>
                                <div>
                                    <v-list-item>
                                        <v-date-picker
                                            v-model="picker"
                                            color="grey darken-3"
                                            class="grey--text"
                                            locale="es-ES"
                                            :events="arrayDate"
                                            >
                                            </v-date-picker>
                                    </v-list-item>
                                </div>
                                <div>
                                    <v-list-item>
                                        <v-img 
                                            :src="require('@/assets/images/image.png')" 
                                            alt="fe" 
                                            class="image"
                                            >
                                            </v-img>
                                    </v-list-item>
                                </div>
                            </v-layout>
                        </v-navigation-drawer>
                        <v-app-bar 
                            :clipped-left="clipped" 
                            fixed 
                            app 
                            color="grey darken-3"
                            >
                                <v-app-bar-nav-icon color="white" @click.stop="drawer=!drawer"/>
                                <v-toolbar-title v-if="!drawer" v-text="title" class="white--text"/>
                                <v-spacer/>
                                <v-text-field
                                    v-model="search"         
                                    append-icon="mdi-magnify"
                                    label="Buscar tarea"
                                    single-line
                                    hide-details
                                    background-color="white"
                                    outlined
                                    dense
                                ></v-text-field>
                        </v-app-bar>
                        
                        <v-col 
                            align="center"
                            class="mt-10"
                            v-for="header in headers"
                            :key="header.id"
                            :title="header.title"
                        >
                            <v-card class="pa-1 white--text" align="left" min-height="50" max-width="400" min-width="250" color="grey darken-3">
                                <v-card-title>{{header.title}}</v-card-title>
                                <div v-for="task in filteredTasks(header.title)"
                                    :key="task.id"
                                    :date="task.date"
                                >
                                        <base-card
                                            :task="task"
                                        >
                                        </base-card>
                                </div>
                                
                            </v-card>
                        </v-col>
                    </v-row>
                </v-container>
            </v-main>
        </div>
    </v-app>
</template>
<script>
import BaseCard from '../components/BaseCard.vue';
export default {
  components: {
        BaseCard,
    },
    data() {
        return{
            clipped:false,
            drawer:false,
            fixed:false,
            miniVariant:false,
            title:"Semillero de Talento",
            search:'',
            picker:'',
            headers:[
                {title:'Pendiente'},
                {title:'Completado'}
            ],
            tasks:[
                {
                    title:'Hacer el mandado',
                    status:'Completado',
                    date:'2022-12-15',
                    comments:['En el soriana esta mas barato el pollo'],
                    description:'Pollo, huevos, servilletas, tostadas, frijol y calabaza',
                    tags: ['compras','salir']
                },
                {
                    title:'Hacer ejercicio',
                    status:'Pendiente',
                    date:'2022-12-15',
                    comments: [],
                    description:'30 minutos de cardio',
                    tags: ['casa','salud']

                },
                {
                    title:'Lavar los platos',
                    status:'Pendiente',
                    date:'2022-12-16',
                    comments:[],
                    description:'',
                    tags: ['casa', 'limpieza']
                },
                {
                    title:'Preparar cena de navidad',
                    status:'Pendiente',
                    date:'2022-12-23',
                    comments:['Comprar mantequilla'],
                    description:'Hacer el pavo',
                    tags: ['casa','comida']
                },
                {
                    title:'Estudiar Vue',
                    status:'Completado',
                    date:'2022-12-11',
                    comments:[],
                    description:'Estudiar sobre la jerarquia de padre-hijo',
                    tags: ['casa','trabajo']
                },
                {
                    title:'Alimentar a los gatos',
                    status:'Pendiente',
                    date:'2022-12-23',
                    comments:['Darle su medicina a Isidora'],
                    description:'',
                    tags: ['casa','mascotas']
                },
                {
                    title:'Hacer comida',
                    status:'Pendiente',
                    date:'2022-12-20',
                    comments:[],
                    description:'Preparar teriyaki :)',
                    tags: ['casa','comida']
                },
                {
                    title:'Pagar la luz',
                    status:'Completado',
                    date:'2022-12-20',
                    comments:[],
                    description:'Pagar el servicio por la aplicacion',
                    tags: ['casa']
                },
            ]
        };
    },
    computed: {
        arrayDate() {
        return this.tasks.map(task => task.date)
        }
    },
    methods:{
        filteredTasks(headerTitle){
            if(this.search==''){
                return this.tasks.filter(task => 
                (task.status.includes(headerTitle)&& 
                task.date.includes(this.picker)));
            }
            else{
                return this.tasks.filter(task => 
                (task.status.includes(headerTitle)&& 
                task.date.includes(this.picker)&&
                (task.tags.includes(this.search.toLowerCase())||
                task.title.toLowerCase().includes(this.search.toLowerCase())))
                );
            }
        },
        showAllTasks(){
            this.picker='';
            this.search='';
        }
    },
}
</script>
<style scoped>
.rounded-card{
    border-radius:50px;
}
.content{
    display: flex;
}
</style>
