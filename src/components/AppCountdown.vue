<template>
    <div class="app-countdown-container" @click="pauseCountdown()">
        <time class="app-countdown" :datetime="countdown" :class="{ending: isEnding}">{{countdown}}</time>
        <button ref="start" id="btn-start" @click="start">Start</button>
    </div>
</template>

<script>

export default {
    name: "AppCountdown",
    props: {
        time: {
            type: Number,
            required: true
        },
        paused: {
            type: Boolean
        },
        isOnTheFloor: {
            type: Boolean
        }
    },
    data() {
        return {
            countdown: this.time,
            isPaused: this.paused
        }
    },
    watch: {
        isPaused: function (value) {
            console.log('isPaused',value);
            if (value) {
                this.removeCount();
            }
        },
        countdown: function(value) {
            if (value === 0) {
                this.removeCount();
            }
        }
    },
    computed: {
        isEnding() {
            return this.countdown <= 5;
        }
    },
    methods: {
        pauseCountdown() {
            this.$emit('pause');
        },
        startCount() {
            this.intervalId = setInterval(() => {
                this.countdown -= 1;
                if (this.countdown === 0) this.$emit('finish');
            }, 1000);
        },
        removeCount() {
            
            this.speak(this.countdown);
            clearInterval(this.intervalId);
        },
        start(){
            this.speak('Get ready?');
        },
        speak(txt) {
            const voices = this.synth.getVoices();
            const utterThis = new SpeechSynthesisUtterance(txt);
            utterThis.voice = voices[0];
            utterThis.pitch = 1;
            utterThis.rate = 1;
            this.synth.speak(utterThis);
        }
    },
    created() {
        this.startCount();
        this.synth = window.speechSynthesis;
        console.log(this.synth);
    },
    unmounted() {
        this.removeCount()
    }
};
</script>


<style scoped>
    .app-countdown-container {
        font-size: 500%;
    }
    .app-countdown {
        background-color: rgba(0,0,0,.1);
        border-radius: 5%;
        box-shadow: 0px 3px 3px rgba(0,0,0,.5);
        padding: 15px;
        cursor: pointer;
        color: whitesmoke;
    }
    .app-countdown:active {
        background-color: green;
    }
    .ending {
        color: red;
    }
</style>