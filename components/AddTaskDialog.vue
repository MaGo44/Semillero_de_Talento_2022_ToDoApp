/* 
    Descripcion:
    Componente formulario para agregar una tarea.

    Entrada: --
    Salida: Datos de nueva tarea.
*/
<template>
  <v-row>
    <v-btn justify="right" @click="isOpen=true">Agregar Tarea</v-btn>
    <v-dialog
      v-model="isOpen"
      persistent
      max-width="600px"
    >
      <v-card>
        <v-card-title>
          <span class="text-h5" >Agregar Tarea</span>
        </v-card-title>
        <v-card-text>
          <v-form
            ref="form"
            v-model="valid"
            lazy-validation
          >
            <v-container>
            <v-row>
              <v-col
                cols="12"
              >
                <v-text-field
                  label="Titulo"
                  v-model.trim="title"
                  required
                  :rules="titleRules"
                  :counter="20"
                  color="orange darken-2"
                >
                </v-text-field>
              </v-col>
              <v-col cols="12">
                <v-textarea
                  label="Descripcion"
                  v-model.trim="description"
                  :rules="lengthRules"
                  :counter="200"
                  color="orange darken-2"
                >
                </v-textarea>
              </v-col>
              <v-col
                cols="12"
                sm="6"
                md="4"
              >
                <v-menu
                  ref="menu"
                  v-model="menu"
                  :close-on-content-click="false"
                  :return-value.sync="date"
                  transition="scale-transition"
                  offset-y
                  min-width="auto"
                  color="orange darken-2"
                >
                  <template v-slot:activator="{ on, attrs }">
                    <v-text-field
                      v-model="date"
                      label="Fecha"
                      prepend-icon="mdi-calendar"
                      readonly
                      v-bind="attrs"
                      v-on="on"
                      color="orange darken-2"
                    ></v-text-field>
                  </template>
                  <v-date-picker
                    v-model="date"
                    no-title
                    scrollable
                    color="orange darken-2"
                  >
                    <v-spacer></v-spacer>
                    <v-btn
                      text
                      color="orange darken-3"
                      @click="menu = false"
                    >
                      Cancel
                    </v-btn>
                    <v-btn
                      text
                      color="orange darken-3"
                      @click="$refs.menu.save(date)"
                    >
                      OK
                    </v-btn>
                  </v-date-picker>
                </v-menu>
              </v-col>
              
              <v-col cols="12">
                <v-combobox
                  v-model.trim="chips"
                  chips
                  clearable
                  deletable-chips
                  label="Etiqueta"
                  :rules="tagRules"
                  :counter="10"
                  color="orange darken-2"
                >
                </v-combobox>
              </v-col>
              <v-col cols="12">
                <v-text-field
                  label="Comentario"
                  v-model.trim="comments"
                  :rules="lengthRules"
                  :counter="200"
                  color="orange darken-2"
                  >
                </v-text-field>
              </v-col>
              <v-col cols="12">
                <span v-if="valid==false">
                  Por favor, corregir los campos...
                </span>
              </v-col>
            </v-row>
          </v-container>
          </v-form>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="orange darken-3"
            text
            @click=updateIsOpen()
          >
            Cancelar
          </v-btn>
          <v-btn
            color="orange darken-3"
            text
            @click=submitData()
          >
            Guardar
          </v-btn>
        </v-card-actions>

      </v-card>
    </v-dialog>
  </v-row>
</template>
<script>
import axios from 'axios';
export default {
  data() {
    return {
      isOpen:false,
        title:'',
        description:'',
        chips:'',
        comments:'',
        date: (new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().substr(0, 10),
        menu: false,
        valid: true,
        text:'Hay una tarea nueva!',
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
    updateIsOpen(){
      this.title='',
      this.description='',
      this.chips='',
      this.comments='',
      this.isOpen=false
    },
    async submitData(){
        if(this.$refs.form.validate()){
          const data={
        token:'',
        title:this.title,
        is_completed: 0,
        due_date:this.date,
        comments:this.comments,
        description:this.description,
        tags:this.chips
        }
        console.log(data);
        const params=new URLSearchParams(data);
           const token= 'e864a0c9eda63181d7d65bc73e61e3dc6b74ef9b82f7049f1fc7d9fc8f29706025bd271d1ee1822b15d654a84e1a0997b973a46f923cc9977b3fcbb064179ecd';
           await axios.post('https://ecsdevapi.nextline.mx/vdev/tasks-challenge/tasks', params,{
               headers:{
                   'authorization':`Bearer ${token}`,
                   'content-type': 'application/x-www-form-urlencoded'
               }
           }).then((response)=>{
               if(response.status=="201"){
                  this.$emit('update-tasks',this.text);
                  this.isOpen=false;
               }
               else{
                console.log(response.status);
               }
           })
        }
        
    }
  }
}
</script>