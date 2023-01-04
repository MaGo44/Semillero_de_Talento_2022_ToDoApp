<template>
    <div>
        <v-card-title class="text-h6 font-weight-bold">
            <v-list-item class="grow" align="center">
                <v-row
                    justify="start"
                    align="center"
                >
                    <div v-if="isComplete==1">
                        <v-list-item-avatar color="grey">
                            <v-icon color="white">
                                mdi-check-circle-outline
                            </v-icon>
                        </v-list-item-avatar>
                        <span>
                            Completado
                        </span>
                    </div>
                    <div v-else-if="isComplete==0">
                        <v-list-item-avatar color="grey">
                            <v-icon color="white">
                                mdi-clock-alert-outline
                            </v-icon>
                        </v-list-item-avatar>
                        <span>
                            Pendiente
                        </span>
                    </div>
                </v-row>
                <v-row
                    justify="end"
                    align="center"
                >
                    <v-list-item-avatar color="grey">
                        <v-icon color="white">
                            mdi-calendar-range-outline
                        </v-icon>
                    </v-list-item-avatar>
                    <span v-if="date==null">
                        Sin fecha
                    </span>
                    <span else class="text-h6">
                        {{date}}
                    </span>
                </v-row>
            </v-list-item>
        </v-card-title>
        <v-card-text>
            <v-card-title>
                {{title}}
            </v-card-title>
            <v-card-text v-if="!tag==''">
                <v-chip 
                    color="grey" 
                    class="text-h7 font-weight-light white--text mr-2" 
                >
                    {{tag}}
                </v-chip>
            </v-card-text>
            <v-card-text v-if="description==''">
                Sin descripcion
            </v-card-text>
            <v-card-text else class="text--primary">
                {{description}}
            </v-card-text>
            <v-card-title class="text--primary">
                Comentarios
            </v-card-title>
            <v-card-text v-if="!comments==''">
                <v-card-text class="text--primary">
                    <v-icon color="grey">mdi-chat-processing-outline</v-icon>
                        {{comments}}
                    </v-card-text>
                </v-card-text>
            <v-card-text v-else class="text--primary">No hay comentarios</v-card-text>
            <v-divider class="mx-4"></v-divider>
            <div align="right">
                <v-btn @click=updateIsEditing()>Editar</v-btn>
                <v-btn @click=deleteTaskIdData()>Borrar</v-btn>
            </div>
        </v-card-text>
        
    </div>
</template>
<script>
import axios from 'axios';
export default {
    props:{
        title:{
            type: String,
            required: true
        },
        date:{
            type: String,
            required: false
        },
        tag:{
            type:String,
            required:false
        },
        description:{
            type:String,
            required:false
        },
        comments:{
            type:String,
            required:false
        },
        isComplete:{
            type:Number,
            required:true
        }
    },
    methods:{
        // Cambia el estado de editando
        updateIsEditing(){
            this.$emit('update-is-editing');
        },
        // Metodo delete de la tarea a la API
         async deleteTaskIdData(){
            const token= 'e864a0c9eda63181d7d65bc73e61e3dc6b74ef9b82f7049f1fc7d9fc8f29706025bd271d1ee1822b15d654a84e1a0997b973a46f923cc9977b3fcbb064179ecd';
            // const params = new URLSearchParams([['id', this.$route.params.id]]);
           await axios.delete(`https://ecsdevapi.nextline.mx/vdev/tasks-challenge/tasks/${this.$route.params.id}`,{
                // params:{id:this.$route.params.id},
                headers:{
                    'authorization':`Bearer ${token}`
                }
           }).then((response)=>{
               if(response.status=="201"){
                   console.log("Yes");
                   this.$router.replace('/tasks');
               }
               else{
                console.log(response.status);
                }
           })
        },
    }
}
</script>