/* 
    Descripcion:
    Componente formulario para editar una tarea.

    Entrada: --
    Salida: Datos editados de la tarea.
*/
<template>
    <v-form 
        ref="form"
        v-model="valid"
        lazy-validation
    >
        <v-container>
            <v-list-item class="grow">
                <v-row>
                    <v-checkbox  
                        label="Completado" 
                        v-model="taskStatus" 
                        :true-value="1" 
                        :false-value="0"
                        color="orange darken-2"
                    >
                    </v-checkbox>
                </v-row>
                <v-row
                    align="center"
                    justify="end"
                >
                    <v-col>
                        <v-menu
                            ref="menu"
                            v-model="menu"
                            :close-on-content-click="false"
                            :return-value.sync="taskDate"
                            transition="scale-transition"
                            offset-y
                            min-width="auto"
                            color="orange darken-2"
                        >
                            <template v-slot:activator="{ on, attrs }">
                                <v-text-field
                                v-model="taskDate"
                                label="Fecha"
                                prepend-icon="mdi-calendar"
                                readonly
                                v-bind="attrs"
                                v-on="on"
                                color="orange darken-2"
                                ></v-text-field>
                            </template>
                            <v-date-picker
                                v-model="taskDate"
                                no-title
                                scrollable
                                color="orange darken-2"
                            >
                                <v-spacer></v-spacer>
                                <v-btn
                                    text
                                    @click="menu = false"
                                    color="orange darken-3"
                                >
                                    Cancel
                                </v-btn>
                                <v-btn
                                    text
                                    color="orange darken-3"
                                    @click="$refs.menu.save(taskDate)"
                                >
                                    OK
                                </v-btn>
                            </v-date-picker>
                        </v-menu>
                    </v-col>
                </v-row>
            </v-list-item>
            <v-card-text>
                <v-row
                    align="center"
                    justify="start"
                >
                    <v-text-field
                        label="Titulo"
                        v-model.trim="taskTitle"
                        required
                        :rules="titleRules"
                        :counter="20"
                        color="orange darken-2"
                    >
                    </v-text-field>
                </v-row>
            </v-card-text>
            <v-card-text>
                <v-combobox
                  v-model.trim="taskTags"
                  chips
                  clearable
                  deletable-chips
                  label="Etiquetas"
                  :rules="tagRules"
                  :counter="10"
                  color="orange darken-2"
                >
                </v-combobox>
            </v-card-text>
            <v-textarea
                  label="Descripcion"
                  v-model.trim="taskDesc"
                  :rules="lengthRules"
                  :counter="200"
                  color="orange darken-2"
                >
            </v-textarea>
            <v-card-title class="text--primary">
                Comentarios
            </v-card-title>
            <v-card-text class="text--primary">
                <v-text-field
                    label="Comentario"
                    v-model.trim="taskCom"
                    :rules="lengthRules"
                    :counter="200"
                    color="orange darken-2"
                >
                </v-text-field>
            </v-card-text>
            <v-divider class="mx-4"></v-divider>
            <div align="right">
                <v-btn @click=updateIsEditing()>Cancelar</v-btn>
                <v-btn @click=editTaskIdData()>Guardar</v-btn>
            </div>
        </v-container>
    </v-form>
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
            type:String,
            required: false
        },
        tags:{
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
    data() {
        return{
            valid: false,
            menu: false,
            taskTitle:this.title,
            taskDate:this.date,
            taskTags:this.tags,
            taskDesc:this.description,
            taskCom:this.comments,
            taskStatus:this.isComplete,
            // Reglas para validar los datos como su longitud y si son o no obligatorias
            titleRules: [
                v => !!v || 'El titulo es obligatorio',
                v => v.length <= 20 || 'El titulo debe tener menos de 20 letras',
            ],
            tagRules: [
                v => (v != null && v.length <= 10) || 'La etiqueta debe tener menos de 10 letras',
            ],
            lengthRules: [
                v => (v != null && v.length <= 200)|| 'Has llegado al maximo de 200 letras',
            ],
        }
    },
    methods:{
        // Cambia el estado de editando
        updateIsEditing(){
            this.$emit('update-is-editing');
        },
        // Metodo put de la tarea a la API
        async editTaskIdData(){
            if(this.$refs.form.validate()){
                const data={
                    title:this.taskTitle,
                    is_completed: this.taskStatus,
                    due_date:this.taskDate,
                    comments:this.taskCom,
                    description:this.taskDesc,
                    tags:this.taskTags
                }
                const text="La tarea se ha editado";
                const params=new URLSearchParams(data);
                const token= 'e864a0c9eda63181d7d65bc73e61e3dc6b74ef9b82f7049f1fc7d9fc8f29706025bd271d1ee1822b15d654a84e1a0997b973a46f923cc9977b3fcbb064179ecd';
                await axios.put(`https://ecsdevapi.nextline.mx/vdev/tasks-challenge/tasks/${this.$route.params.id}`,params,{
                        // params:{id:this.$route.params.id},
                        headers:{
                            'authorization':`Bearer ${token}`,
                            'content-type': 'application/x-www-form-urlencoded'
                        }
                }).then((response)=>{
                    if(response.status=="201"){
                        this.$emit('update-task',text);
                    }
                    else{
                        console.log(response.status);
                        }
                })
         }
        },
    }
}
</script>