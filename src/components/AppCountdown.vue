<template>
    <section 
        class="app-countdown-container" 
        @click="pauseCountdown()">
            <h2>{{title}}</h2>
            <time class="app-countdown" 
                :datetime="countdown" 
                :class="{ending: isEnding}">
                    {{countdown}}
            </time>
            <button 
                @click="toggleCount()" 
                class="action-btn">
                    {{countStatus}}
            </button>
    </section>
</template>

<script>

export default {
    name: "AppCountdown",
    props: {
        time: {
            type: Number,
            required: true
        },
        title: {
            type: String
        },
        paused: {
            type: Boolean,
            default: false
        },
        isLayDown: {
            type: Boolean
        }
    },
    data() {
        return {
            countdown: this.time,
            isPaused: this.paused,
            finished: false,
        }
    },
    watch: {
        isPaused: function (value) {
            console.log('isPaused',value);
            if (value) {
                this.removeCount();
            }
        },
        countdown: function(value, oldVal) {
            if (oldVal <= 6) {
                if (value === 0) {
                    this.removeCount();
                    this.finished = true;
                    this.$emit('finish');
                } else {
                    this.speak(value);
                }
            }
        }
    },
    computed: {
        isEnding() {
            return this.countdown <= 5;
        },
        countStatus() {
            return this.isPaused ? "resume" : "pause";
        }
    },
    methods: {
        startCount() {
            this.intervalId = setInterval(() => {
                this.countdown -= 1;
            }, 1000);
        },
        removeCount() {
            clearInterval(this.intervalId);
        },
        toggleCount(){
            if (this.isPaused){
                this.startCount();
                this.$emit('resumed');
            } else {
                this.removeCount();
                this.$emit('paused');
            }
            this.isPaused = !this.isPaused;
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
        if (!this.paused) {
            this.startCount();
        }
        this.synth = window.speechSynthesis;
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
    .action-btn {
        text-transform: Capitalize;
    }
</style>
