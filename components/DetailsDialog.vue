<template>
    <v-dialog
        v-model="dialog"
        persistent
        max-width="500"
    >
        <v-card>
            <v-card-title class="text-h6 font-weight-bold">
                <v-list-item class="grow">
                    <v-row
                        align="center"
                        justify="start"
                    >
                        {{task.title}}
                    </v-row>
                    <v-row
                        align="center"
                        justify="end"
                    >
                        <v-list-item-avatar color="grey">
                            <v-icon color="white">
                                mdi-calendar-range-outline
                            </v-icon>
                        </v-list-item-avatar>
                        <span class="text-h6">
                            {{task.date}}
                        </span>
                    </v-row>
                </v-list-item>
            </v-card-title>
            <v-card-text>
                <v-chip 
                    color="grey" 
                    class="text-h7 font-weight-light white--text mr-2" 
                    v-for="tag in task.tags"
                    :key="tag.id"
                >
                    {{tag}}
                </v-chip>
            </v-card-text>
            <v-card-text class="text--primary">
                {{task.description}}
            </v-card-text>
            <v-divider class="mx-4"></v-divider>
            <v-card-title class="text--primary">
                Comentarios
            </v-card-title>
            <v-card-text v-if="taskComments.length>0">
                <v-card-text class="text--primary" v-for="comment in task.comments"
                    :key="comment.id"
                >
                    <v-icon color="grey">mdi-chat-processing-outline</v-icon>
                        {{comment}}
                </v-card-text>
            </v-card-text>
            <v-card-text v-else class="text--primary">No hay comentarios</v-card-text>
            <v-card-actions class="justify-end">
                <v-btn
                    text
                    @click="dialog=false"
                    class="text--primary"
                >
                    Cerrar
                </v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>
</template>
<script>
export default {
    props:
        ['task', 'dialog']
    ,
    data() {
        return{
            taskComments:this.task.comments
        }
    },

}
</script>