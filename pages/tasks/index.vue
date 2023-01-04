/* 
    Descripcion:
    Pagina para mostrar la lista de tareas y que el usuario es capaz de filtrar segun su fecha y nombre.

    Entrada: Datos de tareas.
    Salida: --
*/
<template>
    <v-app>
        <div>
            <v-main>
                <v-container>
                    <!-- Llama al componente del menu desplegable -->
                        <the-drawer>
                            <template v-slot:drawerContent>
                                <div>
                                    <v-list-item >
                                        <!-- Limpia los filtros y muestra todas las tareas. -->
                                        <v-btn 
                                            @click=showAllTasks()
                                        >
                                            Todas las tareas
                                        </v-btn>
                                    </v-list-item>
                                </div>
                                <div>
                                    <v-list-item>
                                        <!-- Filtro calendario para mostrar tareas por fecha -->
                                        <v-date-picker
                                            v-model="picker"
                                            color="grey darken-3"
                                            class="grey--text"
                                            locale="es-ES"
                                            :events="dates"
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
                                            eager
                                        >
                                        </v-img>
                                    </v-list-item>
                                </div>
                            </template>
                            <template v-slot:barSearcher>
                                <!-- Filtro buscador para mostrar tareas por nombre -->
                                <v-text-field
                                    v-model="search"         
                                    append-icon="mdi-magnify"
                                    label="Buscar tarea"
                                    single-line
                                    hide-details
                                    background-color="white"
                                    outlined
                                    dense
                                    color="orange darken-3"
                                ></v-text-field>
                            </template>
                        </the-drawer>
                    <template>
                        <v-row>
                            <v-col
                                align="left"
                                class="mt-10 mb-5"
                            >
                                <!-- Llama al componente que despliega el formulario para agregar una tarea -->
                                <add-task-dialog
                                    @update-tasks=snackbarAction
                                >
                                </add-task-dialog>
                            </v-col>
                        </v-row>
                    </template>
                    <!-- Se muestra un cargador mientras se reciben los datos de la API -->
                    <v-row v-if="isLoading==true" align="center" justify="center" class="mt-10">
                        <div>
                            <h3>En carga...</h3>
                        </div>
                        <div>
                            <v-img 
                                :src="require('@/assets/images/auxiliar.png')" 
                                alt="fe" 
                                class="image"
                                max-width="200"
                                eager
                            >
                            </v-img>
                        </div>
                    </v-row>
                    <v-row v-else>
                        <!-- Se muestran los encabezados dentro del arreglo de encabezados -->
                        <v-col 
                            align="center"
                            class="mt-10"
                            v-for="header in headers"
                            :key="header.id"
                            :id="header.id"
                            :title="header.title"
                        >
                            <v-card class="pa-1 white--text" align="left" min-height="50" max-width="400" min-width="250" color="grey darken-3">
                                <v-card-title>
                                    {{header.title}}
                                </v-card-title>
                                <!-- Se muestran las tareas -->
                                <div v-for="task in filteredTasks(header.id)"
                                    :key="task.id"
                                    :date="task.due_date"
                                >
                                    <!-- Los datos de las tareas se envuelven en el componente base-->
                                    <base-card
                                        :id="task.id"
                                        :title="task.title"
                                        :date="task.due_date"
                                        :description="task.description"
                                        :comments="task.comments"
                                        :tag="task.tags"
                                    >
                                    </base-card>
                                </div>
                            </v-card>
                        </v-col>
                    </v-row>
                    <!-- Muestra un mensaje cuando se ha creado una tarea exitosamente -->
                    <template>
                        <v-snackbar
                            v-model="isSnackbar"
                            >
                            {{ text }}
                            <template v-slot:action="{ attrs }">
                                <v-btn
                                    color="orange darken-3"
                                    text
                                    v-bind="attrs"
                                    @click="isSnackbar = false"
                                >
                                Ok
                                </v-btn>
                            </template>
                        </v-snackbar>
                    </template>
                </v-container>
            </v-main>
        </div>
    </v-app>
</template>
<script>
import axios from 'axios';
import BaseCard from '../../components/BaseCard.vue';
import TheDrawer from '../../components/TheDrawer.vue';
import AddTaskDialog from '../../components/AddTaskDialog.vue';
export default {
  components: {
        BaseCard,
        TheDrawer,
        AddTaskDialog,
    },
    data() {
        return{
            isClipped:false,
            isOpen:false,
            isFixed:false,
            isActive:false,
            isSnackbar:false,
            isLoading:true,
            text:'',
            title:"Semillero de Talento",
            search:'',
            picker:'',
            headers:[
                {
                    id:0,
                    title:'Pendiente'
                },
                {
                    id:1,
                    title:'Completado'
                }
            ],
            tasks:[]
        };
    },
    // Recupera las fechas de cada tarea dentro del arreglo de tareas y las guarda en un arreglo
    computed: {
        dates() {
            return this.tasks.map(task => task.due_date)
        }
    },
    methods:{
        snackbarAction(text){
            this.getTaskData();
            this.isSnackbar=true;
            this.text=text;
        },
        // Filtra las tareas por fecha y nombre recibido asi como el estado de completado
        filteredTasks(headerId){
            if(this.picker==''){
                return this.tasks.filter(task => 
                (task.is_completed.toString().includes(headerId))&&
                task.title.toLowerCase().includes(this.search.toLowerCase()));
            }
            else
            return this.tasks.filter(task => 
                (task.is_completed.toString().includes(headerId)&&
                task.title.toLowerCase().includes(this.search.toLowerCase())&& 
                (task.due_date==(this.picker))));
        },
        // Limpia los filtros y muestra todas las tareas.
        showAllTasks(){
            this.picker='';
            this.search='';
        },
        // Metodo get de las tareas a la API
        async getTaskData(){
           const token= 'e864a0c9eda63181d7d65bc73e61e3dc6b74ef9b82f7049f1fc7d9fc8f29706025bd271d1ee1822b15d654a84e1a0997b973a46f923cc9977b3fcbb064179ecd';
           await axios.get('https://ecsdevapi.nextline.mx/vdev/tasks-challenge/tasks',{
               headers:{
                   'authorization':`Bearer ${token}`
               }
           }).then((response)=>{
               if(response.status=="200"){
                    this.tasks=response.data;
                    this.isLoading=false;
               }
               else{
                console.log(response.status);
                this.isLoading=true;
                }
           })
       }
    },
    // Inicializa la pagina llamando al metodo 
    created(){
        this.getTaskData();
    }
}
</script>