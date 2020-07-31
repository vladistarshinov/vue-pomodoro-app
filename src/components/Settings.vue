<template>
    <v-dialog 
        v-model="modal" 
        persistent 
        max-width="600px"
    >
        <v-card>
            <v-card-title>
                <span class="headline">Settings</span>
            </v-card-title>
            <v-card-text>
                <v-text-field
                    v-model="updateTimers[index].minutesCount"
                    v-for="(timer, index) in updateTimers"
                    :key="index"
                    :label="updateTimers[index].title"
                    type="number"
                ></v-text-field>
            </v-card-text>
            <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn 
                    color="blue darken-1" 
                    text 
                    outlined
                    @click="closeModal"
                >Close</v-btn>
                <v-btn 
                    color="blue darken-1" 
                    text 
                    outlined
                    @click="saveSettings(updateTimers)"
                >Save</v-btn>
        </v-card-actions>
        </v-card>
    </v-dialog>
</template>

<script>
    export default {
        props: {
            modal: {
               type: Boolean,
               required: true
            },
            closeModal: {
                type: Function,
                required: true
            },
            saveSettings: {
                type: Function,
                required: true
            },
            timers: {
                type: Array,
                required: true
            }
        },
        data(){
            return {
                updateTimers: []
            }
        },
        mounted(){
            this.updateTimers = this.timers.map((timer) => {
                return timer
            })
        }
    }
</script>

<style lang="sass" scoped>

</style>