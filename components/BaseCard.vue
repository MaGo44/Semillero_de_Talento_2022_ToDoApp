<template>
    <div>
        <v-hover v-slot="{ hover }">
                <v-card
                    @click="dialog=true"
                    :elevation="hover ? 12 : 2"
                    class="mx-auto {'on-hover': hover} mb-10"
                    max-width="400"
                >
                    <v-card-text>
                        <v-chip 
                            color="grey" 
                            column 
                            class="text-h7 font-weight-light white--text mr-2" 
                            v-for="tag in task.tags"
                            :key="tag.id"
                        >
                        {{tag}}
                        </v-chip>
                    </v-card-text>

                    <v-card-title class="text-h6 font-weight-bold">
                    {{task.title}}
                    </v-card-title>
                    <v-card-text 
                        v-if="!shortenedDescription==''" 
                        class="text-h7">{{ shortenedDescription }}</v-card-text>
                    <v-card-actions>
                        <v-list-item class="grow">
                            <v-list-item-avatar color="grey">
                                <v-icon color="white">mdi-calendar-range-outline</v-icon>
                            </v-list-item-avatar>
                            <v-list-item-content>
                                <v-list-item-title class="text-h7">{{task.date}}</v-list-item-title>
                            </v-list-item-content>
                            <v-row
                                align="center"
                                justify="end"
                            >
                                <span class="subheading mr-2">{{taskComments.length}}</span>
                                <v-icon color="grey">
                                    mdi-comment-text-outline
                                </v-icon>
                            </v-row>
                        </v-list-item>
                    </v-card-actions>
                </v-card>
        </v-hover>
        <template>
            <v-row justify="center">
                <details-dialog :dialog="dialog" :task="this.task">

                </details-dialog>
            </v-row>
        </template>
    </div>
</template>
<script>
import DetailsDialog from './DetailsDialog.vue';
export default {
  components: { DetailsDialog },
    props:{
        task:{
            type:Object,
            require:true
        }
    },
    data(){
        return{
            dialog:false,
            taskComments:this.task.comments,
            taskDescription:this.task.description
        };
    },
    computed:{
        shortenedDescription(){
            const maxlength=45;
            if(this.taskDescription.length <= maxlength){
                return this.taskDescription;
            }
            else{
                return this.taskDescription.substr(0,maxlength-10)+'...Ver mas.'
            }
        }
    }
}
</script>
<style>
.v-card{
    background-color: rgb(117, 75, 75);
    transition: background-color 1000ms linear;
}
.v-card:not(.on-hover) {
    background-color: white;
 }
</style>