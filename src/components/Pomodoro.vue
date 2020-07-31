<template>
    <v-card class="mt-8">
        <v-tabs 
            @change="changeTimer" 
            v-model="currentTimer" 
            grow
        >
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
                <v-btn @click="reset(this.timers[currentTimer].minutesCount)" v-else :disabled="isRunning">
                    <v-icon left small >mdi-restart</v-icon>
                    Reset
                </v-btn>
            </v-row>
            <v-row class="wrapper d-flex justify-center">
                <v-chip
                    class="ma-2"
                    color="green"
                    text-color="white"
                >
                    <v-avatar
                        left
                        class="green darken-2"
                    >
                    {{countCycles}}
                    </v-avatar>
                    Work cycles
                </v-chip>
            </v-row>
        </v-card>

        <Settings 
            :modal="modal" 
            :closeModal="closeModal" 
            :saveSettings="saveSettings" 
            :timers="timers"
        />
    </v-card>
</template>

<script>
import Settings from './Settings'
export default {
    components: {
        Settings 
    },
    props: {
        modal: {
            type: Boolean,
            required: true
        },
        closeModal: {
            type: Function,
            required: true
        }
    },
    data(){
        return {
            countStartPush: 0,
            countCycles: 0, 
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
                if (this.totalSeconds <= 0) {
                    this.stop() 
                    this.countStartPush++
                    if (this.countStartPush % 2 == 0 && this.countStartPush !== 0) {
                        this.countCycles++
                    }
                    this.selectTimer()
                    return
                }
                this.totalSeconds--
            }, 1000)  
        },
        stop(){
            this.isRunning = false
            clearInterval(this.timerInstance)
        },
        reset(minutesCount){
            this.stop()
            this.totalSeconds = minutesCount * 60
        },
        selectTimer(){
            if (this.countStartPush == 8) {
                this.changeTimer(this.currentTimer + 1)
                this.countStartPush = -1
            }
            else {
                if (this.currentTimer == 0)
                    this.changeTimer(this.currentTimer + 1)
                else if (this.currentTimer == 1)
                    this.changeTimer(this.currentTimer - 1)
                else 
                    this.changeTimer(this.currentTimer - 2)
            }            
            this.start()
            return
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
        },
        saveSettings(updateTimers){
            this.timers = this.timers.map((timer, index) => {
                return {...timer, minutesCount: parseInt(updateTimers[index].minutesCount)}
            })
            this.closeModal()
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