<template>
    <v-card class="mt-8">
        <v-tabs @change="changeTimer" v-model="currentTimer" grow>
            <v-tab
                v-for="timer in timers"
                :key="timer.title"
            >
            {{ timer.title }}
            </v-tab>
        </v-tabs>

        <v-card
            class="pa-5 d-flex flex-column justify-center align-center"
            color="basil"
            flat
        >
            <h1 class="display">{{ displayMinutes }}:{{ displaySeconds }}</h1>
            <v-row class="wrapper d-flex justify-center">
                <v-btn @click="start" color="primary">
                    <v-icon left small>mdi-play-circle-outline</v-icon>
                    Start
                </v-btn>
                <v-btn @click="stop" color="error">
                    <v-icon left small>mdi-stop-circle-outline</v-icon>
                    Stop
                </v-btn>
                <v-btn @click="reset(timers[currentTimer].minutesCount)" dark v-if="!isRunning" :disabled="isRunning" color="grey">
                    <v-icon left small >mdi-restart</v-icon>
                    Reset
                </v-btn>
                <v-btn @click="reset(timers[currentTimer].minutesCount)" v-else :disabled="isRunning">
                    <v-icon left small >mdi-restart</v-icon>
                    Reset
                </v-btn>
            </v-row>
        </v-card>
    </v-card>
</template>

<script>
export default {
    data(){
        return {
            isRunning: false,
            timerInstance: null,
            totalSeconds: 25 * 60,
            currentTimer: 0,
            timers: [
                {
                    title: 'Pomodoro',
                    minutesCount: 25
                }, 
                {
                    title: 'Short Break',
                    minutesCount: 5
                }, 
                {
                    title: 'Long Break',
                    minutesCount: 10
                }
            ] 
        }
    },
    computed: {
        displayMinutes(){
            return this.formatTime(Math.floor(this.totalSeconds / 60))
        },
        displaySeconds(){
            return this.formatTime(this.totalSeconds % 60)  
        }
    },
    methods: {
        start(){
            this.stop()
            this.isRunning = true
            this.timerInstance = setInterval(() => {
                this.totalSeconds -= 1
            }, 1000)
        },
        stop(){
            this.isRunning = false
            clearInterval(this.timerInstance)
        },
        reset(minutes){
            this.stop()
            this.totalSeconds = minutes * 60
        },
        formatTime(time){
             if (time < 10) {
                return '0' + time
            }
            return time.toString()
        },
        changeTimer(number) {
            this.currentTimer = number
            this.reset(this.timers[number].minutesCount)
            console.log(this.timers[number].minutesCount)
        }
    }
}
</script>

<style lang="sass" scoped>

.v-card
    width: 600px
.v-btn
    margin: 0 5px
.display
    font-size: 80px
    font-weight: 500
    text-align: center


</style>