/* 
    Descripcion:
    Pagina para mostrar la tarea seleccionada y permite al usuario eliminarla o editarla.

    Entrada: Datos de la tarea.
    Salida: Datos editados de la tarea
*/
<template>
    <v-app>
        <!-- El componente de encabezado -->
        <TheHeader/>
        <v-container>
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
            <div v-else align="center" justify="center">
                <v-card
                    max-width="500" class="mt-10" align="left">
                    <!-- Cuando no se esta editando se muestran los datos de la tarea -->
                    <div v-if="!isEditing">
                        <task-card v-for="task in task"
                            :key="task.id"
                            :title="task.title"
                            :date="task.due_date"
                            :description="task.description"
                            :comments="task.comments"
                            :tag="task.tags"
                            :isComplete="task.is_completed"
                            @update-is-editing="updateIsEditing"
                        >
                        </task-card>
                    </div>
                    <!-- Cuando se esta editando se llama el formulario -->
                    <div v-if="isEditing">
                        <edit-task-card v-for="task in task"
                            :key="task.id"
                            :title="task.title"
                            :date="task.due_date"
                            :description="task.description"
                            :comments="task.comments"
                            :tags="task.tags"
                            :isComplete="task.is_completed"
                            @update-is-editing="updateIsEditing"
                            @update-task="snackbarAction">
                        </edit-task-card>
                    </div>
                </v-card>
            </div>
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
    </v-app>
</template>
<script>
import axios from 'axios';
export default {
    data() {
        return{
            task:[],
            isEditing:false,
            isSnackbar:false,
            isLoading:true,
            text:'',
        }
    },
    methods:{
        // Para habilitar la edicion 
        updateIsEditing(){
            this.isEditing=!this.isEditing;
        },
        // Para actualizar la tarea
        snackbarAction(text){
            this.getTaskIdData();
            this.isSnackbar=true;
            this.text=text;
            this.isEditing=false;
        },
        // Metodo get de la tarea por Id a la API
        async getTaskIdData(){
            const token= 'e864a0c9eda63181d7d65bc73e61e3dc6b74ef9b82f7049f1fc7d9fc8f29706025bd271d1ee1822b15d654a84e1a0997b973a46f923cc9977b3fcbb064179ecd';
            const params = new URLSearchParams([['id', this.$route.params.id]]);
           await axios.get(`https://ecsdevapi.nextline.mx/vdev/tasks-challenge/tasks/${this.$route.params.id}`,{
                // params:{id:this.$route.params.id},
                headers:{
                    'authorization':`Bearer ${token}`
                }
           }).then((response)=>{
               if(response.status=="200"){
                   this.task=response.data;
                   this.isLoading=false;
                   console.log(this.task)
               }
               else(response.status)
           })
        },
    },
    // Inicializa la pagina llamando al metodo 
    created(){
        this.getTaskIdData();
    }
}
</script>