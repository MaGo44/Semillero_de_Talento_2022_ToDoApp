<template>
    <div>
        <v-hover v-slot="{ hover }">
                <v-card
                    nuxt
                    :to="`tasks/${id}`"
                    :elevation="hover ? 12 : 2"
                    class="mx-auto {'on-hover': hover} mb-10"
                    max-width="500"
                >
                    <v-card-text v-if="!tag==''||!tag==null" >
                        <v-chip 
                            color="grey" 
                            column 
                            class="text-h7 font-weight-light white--text mr-2" 
                        >
                        {{tag}}
                        </v-chip>
                    </v-card-text>

                    <v-card-title class="text-h6 font-weight-bold">
                    {{title}}
                    </v-card-title>
                    <v-card-text 
                        v-if="!description==''||!description==null" 
                        class="text-h7">{{ description }}</v-card-text>
                    <v-card-actions>
                        <v-list-item class="grow">
                            <v-list-item-avatar color="grey">
                                <v-icon color="white">mdi-calendar-range-outline</v-icon>
                            </v-list-item-avatar>
                            <v-list-item-content>
                                <v-list-item-title v-if="date==''||date==null" class="text-h7">Sin fecha</v-list-item-title>
                                <v-list-item-title v-else class="text-h7">{{date}}</v-list-item-title>
                            </v-list-item-content>
                            <v-row
                                align="center"
                                justify="end"
                            >
                                <span v-if="!comments==''" class="subheading mr-2">1</span>
                                <span v-else class="subheading mr-2">0</span>
                                <v-icon color="grey">
                                    mdi-comment-text-outline
                                </v-icon>
                            </v-row>
                        </v-list-item>
                    </v-card-actions>
                </v-card>
        </v-hover>
        <!-- <template>
            <v-row justify="center">
                <details-dialog :isOpen="isOpen" :task="this.task" @update-is-open="updateIsOpen">

                </details-dialog>
            </v-row>
        </template> -->
    </div>
</template>

<script>

export default {
    props:{
        id:{
            type: Number,
            required: true
        },
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
        }
    },
    data(){
        return{
            isOpen:false,
            // taskComments:comments,
            // taskDescription:description
        };
    },
    // // computed:{
    //     shortenedDescription(){
    //         const maxlength=45;
    //         if(this.taskDescription.length <= maxlength){
    //             return this.taskDescription;
    //         }
    //         else{
    //             return this.taskDescription.substr(0,maxlength-10)+'...Ver mas.'
    //         }
    //     }
    // },
    methods:{
        updateIsOpen(){
            this.isOpen=!this.isOpen;
        }
    },
    created(){
        console.log(this.tag);
        console.log(this.comments);
        console.log(this.id);
        console.log(this.description);
    }
}
</script>
